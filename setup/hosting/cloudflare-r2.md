---
title: Cloudflare R2 as S3 Bucket
description: How to use Cloudflare's R2 as an S3 bucket for FoundryVTT
published: true
date: 2023-12-17T02:48:11.296Z
tags: s3, cloudflare
editor: markdown
dateCreated: 2023-12-17T02:48:11.296Z
---

> Page is known to be up to date with Core Version 11
{.is-info}

# Cloudflare R2 as S3 Bucket

This is a guide for using Cloudflare R2 as a S3 Bucket for FoundryVTT. We will be using a couple of different Cloudflare technologies to overcome the lack of support in FoundryVTT for alternative S3 providers. You could probably use this approach if you use Cloudflare as your DNS provider with other S3 providers. 

Everything described here is doable with a Cloudflare free tier account.

## Why R2?
"Cloudflare R2 Storage allows developers to store large amounts of unstructured data without the costly egress bandwidth fees associated with typical cloud storage services." - https://developers.cloudflare.com/r2/

For free tier users this is up to 10 GB of storage, plenty of storage for our custom assets in FoundryVTT. If you are already using Cloudflare as your CDN or DNS provider for your FoundryVTT server, this is free storage.

## Built With
- Cloudflare R2
- Cloudflare Workers
- Cloudflare Rewrite URL Rules

## Prerequisites
- Cloudflare Account
- Domain hosted on Cloudflare

## Setup R2 Bucket
### Create Bucket
The first step of our journey is making our bucket. Go to your accounts main page and create a bucket from the R2 tab. I recommend naming it `foundry` so you have to change as little as possible later.

### Create R2 Custom Domains
We need 2 domains for our R2 bucket because of the way that S3 works normally. These domains need to exist before you can assign them to the R2.

First we are going to go to our account and then to the website where our domain is hosted. Then we are going to go to the DNS settings tab and create 2 CNAME records that point at our domain.
| Type  | Name       | Target      |
| ----- | ---------- | ----------- |
| CNAME | s3         | example.com |
| CNAME | foundry.s3 | example.com |

Next head back to the R2 setting page and setup Public Access. You are going to enter the full domain for each of the CNAME records above.
`s3.example.com`
`foundry.s3.example.com`

We could get away with only the last one, but as explained later we want to reduce the number of times our worker could get run as much as possible on the free tier.

### Create CORS policy
We need a CORS policy so that our users can load from our bucket that is hosted on a different subdomain.

Create a CORS policy under the R2 bucket settings under CORS Policy -> Add CORS Policy. You can replace the template there with the follow policy and then click save.
```
[
  {
    "AllowedOrigins": [
      "*"
    ],
    "AllowedMethods": [
      "GET",
      "POST",
      "HEAD"
    ],
    "AllowedHeaders": [
      "*"
    ],
    "ExposeHeaders": [],
    "MaxAgeSeconds": 3000
  }
]
```
Head to your Cloudflare account main page and create an R2 bucket. If you want to keep things simple just name it `foundry`.

### Create R2 API Tokens
We need to create API tokens to access our R2 bucket via the S3 compatability API.

Head back to your accounts main page to the R2 tab once again. Look on the far right for `Manage R2 API Tokens` or just search for it with crtl + f. In here you are going to `Create API Token` with the `Admin Read & Write` permissions. You can lock it down to just the foundry bucket and your server IP address here if you want to.

Make sure you record the credentials under the headings `Access Key ID` and `Secret Access Key`

## Setup FoundryVTT S3 Configuration
You need to configure FoundryVTT to load your S3 configuration. I use the [felddy docker](https://hub.docker.com/r/felddy/foundryvtt) image. Add the following line to your environment variable either in your docker-compose.yml or docker run command.
```FOUNDRY_AWS_CONFIG=s3.json```

Then create /foundrydata/Config/s3.json and fill it in with the `Access Key ID` and `Secret Access Key` you got from the previous step.
```
{
  "region": "us-east-1",
  "endpoint": "https://s3.example.com",
  "credentials": {
    "accessKeyId": "SPACES_KEY",
    "secretAccessKey": "SPACES_SECRET"
  }
}
```

## Setup Worker
We are going to use the magic of edge compute to now both use the same s3 subdomain for public bucket access and for connecting to the S3 API. We have to do this since the S3 API FoundryVTT is expecting is different than the R2 API Cloudflare provides. FoundryVTT expects 3 things from the single `endpoint` provided in the configuration, but the endpoints we need to hit for Cloudflare are completely different.

|   | FoundryVTT S3 | Cloudflare R2
| --- | --- | --- |
| Query for buckets | https://s3.example.com | https://<ACCOUNT_ID>.r2.cloudflarestorage.com |
| Query bucket content | https://<BUCKET_NAME>.s3.example.com | https://<BUCKET_NAME>.<ACCOUNT_ID>.r2.cloudflarestorage.com |
| Public access | https://<BUCKET_NAME>.s3.example.com | https://<BUCKET_NAME>.s3.example.com |

The other wrinkle we have is that we can't just pull proxy shenanigans as multiple parts of the signed requests (including hostname and url) are included in the HMAC authentication used to give us access to our bucket.

We also don't want our worker running for the public access requests from all our users, which is where the URL rewrites come in (how to setup these is shown in another step).

So with all this information, what do we need to do in our worker?
- Check to see if the request contains the `authorization` http header. Reject the request if it isn't in our request.
- Check to see which domain this request is from and rewrite the URL
- Remove our URL rewrite trickery
- Generate a new request and remove headers that will mess with our HMAC signature
- Use AwsV4Signer to check the current request to make sure it is valid (against the original domain and url). Reject the request if the signatures don't match.
- Use AwsV4Signer to sign our request our Cloudflare S3 compatability API.
- fetch()

Now head to your Cloudflare accounts main page and go to the `Workers & Pages` tab. Click `Create Application` and then `Create Worker`. Name it whatever you want and then go to configure it.

The first thing we need to do is setup our routes so that it runs at the right times. Hit the view link under routes (or go to the triggers tab) and scroll down to routes. We can't change the default route that is at workers.dev. We are going to do this 3 times for each of our routes. Click `Add route` and then add the following routes.
- s3.example.com/
- foundry.s3.example.com/
- foundry.s3.example.com/worker/*

These routes will run our worker only when we browse to the root our subdomains -OR- for any URL in the worker/ path for the bucket subdomain.

Now we are going to insert our worker code. You can click the `Quick edit` and copy and paste the entire worker below. Make sure you replace the following in the worker script with your information. You can find your account ID on the main Cloudflare account landing page.
```
    const my_s3_domain = "s3.example.com";
    const my_bucket_name = "foundry"
    const my_account_id = "00000000000000000000000000000000";
    const my_access_key = "00000000000000000000000000000000";
    const my_secret_key = "0000000000000000000000000000000000000000000000000000000000000000";
```

#### worker.js
```
/**
 * @license MIT <https://opensource.org/licenses/MIT>
 * @copyright Michael Hart 2022
 * https://unpkg.com/aws4fetch@1.0.17/dist/aws4fetch.esm.js
 */
const encoder = new TextEncoder();
const HOST_SERVICES = {
  appstream2: 'appstream',
  cloudhsmv2: 'cloudhsm',
  email: 'ses',
  marketplace: 'aws-marketplace',
  mobile: 'AWSMobileHubService',
  pinpoint: 'mobiletargeting',
  queue: 'sqs',
  'git-codecommit': 'codecommit',
  'mturk-requester-sandbox': 'mturk-requester',
  'personalize-runtime': 'personalize',
};
const UNSIGNABLE_HEADERS = new Set([
  'authorization',
  'user-agent',
  'presigned-expires',
  'expect',
  'x-amzn-trace-id',
  'range',
  'connection',
]);
class AwsClient {
  constructor({ accessKeyId, secretAccessKey, sessionToken, service, region, cache, retries, initRetryMs }) {
    if (accessKeyId == null) throw new TypeError('accessKeyId is a required option')
    if (secretAccessKey == null) throw new TypeError('secretAccessKey is a required option')
    this.accessKeyId = accessKeyId;
    this.secretAccessKey = secretAccessKey;
    this.sessionToken = sessionToken;
    this.service = service;
    this.region = region;
    this.cache = cache || new Map();
    this.retries = retries != null ? retries : 10;
    this.initRetryMs = initRetryMs || 50;
  }
  async sign(input, init) {
    if (input instanceof Request) {
      const { method, url, headers, body } = input;
      init = Object.assign({ method, url, headers }, init);
      if (init.body == null && headers.has('Content-Type')) {
        init.body = body != null && headers.has('X-Amz-Content-Sha256') ? body : await input.clone().arrayBuffer();
      }
      input = url;
    }
    const signer = new AwsV4Signer(Object.assign({ url: input }, init, this, init && init.aws));
    const signed = Object.assign({}, init, await signer.sign());
    delete signed.aws;
    try {
      return new Request(signed.url.toString(), signed)
    } catch (e) {
      if (e instanceof TypeError) {
        return new Request(signed.url.toString(), Object.assign({ duplex: 'half' }, signed))
      }
      throw e
    }
  }
  async fetch(input, init) {
    for (let i = 0; i <= this.retries; i++) {
      const fetched = fetch(await this.sign(input, init));
      if (i === this.retries) {
        return fetched
      }
      const res = await fetched;
      if (res.status < 500 && res.status !== 429) {
        return res
      }
      await new Promise(resolve => setTimeout(resolve, Math.random() * this.initRetryMs * Math.pow(2, i)));
    }
    throw new Error('An unknown error occurred, ensure retries is not negative')
  }
}
class AwsV4Signer {
  constructor({ method, url, headers, body, accessKeyId, secretAccessKey, sessionToken, service, region, cache, datetime, signQuery, appendSessionToken, allHeaders, singleEncode }) {
    if (url == null) throw new TypeError('url is a required option')
    if (accessKeyId == null) throw new TypeError('accessKeyId is a required option')
    if (secretAccessKey == null) throw new TypeError('secretAccessKey is a required option')
    this.method = method || (body ? 'POST' : 'GET');
    this.url = new URL(url);
    this.headers = new Headers(headers || {});
    this.body = body;
    this.accessKeyId = accessKeyId;
    this.secretAccessKey = secretAccessKey;
    this.sessionToken = sessionToken;
    let guessedService, guessedRegion;
    if (!service || !region) {
[guessedService, guessedRegion] = guessServiceRegion(this.url, this.headers);
    }
    this.service = service || guessedService || '';
    this.region = region || guessedRegion || 'us-east-1';
    this.cache = cache || new Map();
    this.datetime = datetime || new Date().toISOString().replace(/[:-]|\.\d{3}/g, '');
    this.signQuery = signQuery;
    this.appendSessionToken = appendSessionToken || this.service === 'iotdevicegateway';
    this.headers.delete('Host');
    if (this.service === 's3' && !this.signQuery && !this.headers.has('X-Amz-Content-Sha256')) {
      this.headers.set('X-Amz-Content-Sha256', 'UNSIGNED-PAYLOAD');
    }
    const params = this.signQuery ? this.url.searchParams : this.headers;
    params.set('X-Amz-Date', this.datetime);
    if (this.sessionToken && !this.appendSessionToken) {
      params.set('X-Amz-Security-Token', this.sessionToken);
    }
    this.signableHeaders = ['host', ...this.headers.keys()]
      .filter(header => allHeaders || !UNSIGNABLE_HEADERS.has(header))
      .sort();
    this.signedHeaders = this.signableHeaders.join(';');
    this.canonicalHeaders = this.signableHeaders
      .map(header => header + ':' + (header === 'host' ? this.url.host : (this.headers.get(header) || '').replace(/\s+/g, ' ')))
      .join('\n');
    this.credentialString = [this.datetime.slice(0, 8), this.region, this.service, 'aws4_request'].join('/');
    if (this.signQuery) {
      if (this.service === 's3' && !params.has('X-Amz-Expires')) {
        params.set('X-Amz-Expires', '86400');
      }
      params.set('X-Amz-Algorithm', 'AWS4-HMAC-SHA256');
      params.set('X-Amz-Credential', this.accessKeyId + '/' + this.credentialString);
      params.set('X-Amz-SignedHeaders', this.signedHeaders);
    }
    if (this.service === 's3') {
      try {
        this.encodedPath = decodeURIComponent(this.url.pathname.replace(/\+/g, ' '));
      } catch (e) {
        this.encodedPath = this.url.pathname;
      }
    } else {
      this.encodedPath = this.url.pathname.replace(/\/+/g, '/');
    }
    if (!singleEncode) {
      this.encodedPath = encodeURIComponent(this.encodedPath).replace(/%2F/g, '/');
    }
    this.encodedPath = encodeRfc3986(this.encodedPath);
    const seenKeys = new Set();
    this.encodedSearch = [...this.url.searchParams]
      .filter(([k]) => {
        if (!k) return false
        if (this.service === 's3') {
          if (seenKeys.has(k)) return false
          seenKeys.add(k);
        }
        return true
      })
      .map(pair => pair.map(p => encodeRfc3986(encodeURIComponent(p))))
      .sort(([k1, v1], [k2, v2]) => k1 < k2 ? -1 : k1 > k2 ? 1 : v1 < v2 ? -1 : v1 > v2 ? 1 : 0)
      .map(pair => pair.join('='))
      .join('&');
  }
  async sign() {
    if (this.signQuery) {
      this.url.searchParams.set('X-Amz-Signature', await this.signature());
      if (this.sessionToken && this.appendSessionToken) {
        this.url.searchParams.set('X-Amz-Security-Token', this.sessionToken);
      }
    } else {
      this.headers.set('Authorization', await this.authHeader());
    }
    return {
      method: this.method,
      url: this.url,
      headers: this.headers,
      body: this.body,
    }
  }
  async authHeader() {
    return [
      'AWS4-HMAC-SHA256 Credential=' + this.accessKeyId + '/' + this.credentialString,
      'SignedHeaders=' + this.signedHeaders,
      'Signature=' + (await this.signature()),
    ].join(', ')
  }
  async signature() {
    const date = this.datetime.slice(0, 8);
    const cacheKey = [this.secretAccessKey, date, this.region, this.service].join();
    let kCredentials = this.cache.get(cacheKey);
    if (!kCredentials) {
      const kDate = await hmac('AWS4' + this.secretAccessKey, date);
      const kRegion = await hmac(kDate, this.region);
      const kService = await hmac(kRegion, this.service);
      kCredentials = await hmac(kService, 'aws4_request');
      this.cache.set(cacheKey, kCredentials);
    }
    return buf2hex(await hmac(kCredentials, await this.stringToSign()))
  }
  async stringToSign() {
    return [
      'AWS4-HMAC-SHA256',
      this.datetime,
      this.credentialString,
      buf2hex(await hash(await this.canonicalString())),
    ].join('\n')
  }
  async canonicalString() {
    return [
      this.method.toUpperCase(),
      this.encodedPath,
      this.encodedSearch,
      this.canonicalHeaders + '\n',
      this.signedHeaders,
      await this.hexBodyHash(),
    ].join('\n')
  }
  async hexBodyHash() {
    let hashHeader = this.headers.get('X-Amz-Content-Sha256') || (this.service === 's3' && this.signQuery ? 'UNSIGNED-PAYLOAD' : null);
    if (hashHeader == null) {
      if (this.body && typeof this.body !== 'string' && !('byteLength' in this.body)) {
        throw new Error('body must be a string, ArrayBuffer or ArrayBufferView, unless you include the X-Amz-Content-Sha256 header')
      }
      hashHeader = buf2hex(await hash(this.body || ''));
    }
    return hashHeader
  }
}
async function hmac(key, string) {
  const cryptoKey = await crypto.subtle.importKey(
    'raw',
    typeof key === 'string' ? encoder.encode(key) : key,
    { name: 'HMAC', hash: { name: 'SHA-256' } },
    false,
    ['sign'],
  );
  return crypto.subtle.sign('HMAC', cryptoKey, encoder.encode(string))
}
async function hash(content) {
  return crypto.subtle.digest('SHA-256', typeof content === 'string' ? encoder.encode(content) : content)
}
function buf2hex(buffer) {
  return Array.prototype.map.call(new Uint8Array(buffer), x => ('0' + x.toString(16)).slice(-2)).join('')
}
function encodeRfc3986(urlEncodedStr) {
  return urlEncodedStr.replace(/[!'()*]/g, c => '%' + c.charCodeAt(0).toString(16).toUpperCase())
}
function guessServiceRegion(url, headers) {
  const { hostname, pathname } = url;
  if (hostname.endsWith('.r2.cloudflarestorage.com')) {
    return ['s3', 'auto']
  }
  if (hostname.endsWith('.backblazeb2.com')) {
    const match = hostname.match(/^(?:[^.]+\.)?s3\.([^.]+)\.backblazeb2\.com$/);
    return match != null ? ['s3', match[1]] : ['', '']
  }
  const match = hostname.replace('dualstack.', '').match(/([^.]+)\.(?:([^.]*)\.)?amazonaws\.com(?:\.cn)?$/);
  let [service, region] = (match || ['', '']).slice(1, 3);
  if (region === 'us-gov') {
    region = 'us-gov-west-1';
  } else if (region === 's3' || region === 's3-accelerate') {
    region = 'us-east-1';
    service = 's3';
  } else if (service === 'iot') {
    if (hostname.startsWith('iot.')) {
      service = 'execute-api';
    } else if (hostname.startsWith('data.jobs.iot.')) {
      service = 'iot-jobs-data';
    } else {
      service = pathname === '/mqtt' ? 'iotdevicegateway' : 'iotdata';
    }
  } else if (service === 'autoscaling') {
    const targetPrefix = (headers.get('X-Amz-Target') || '').split('.')[0];
    if (targetPrefix === 'AnyScaleFrontendService') {
      service = 'application-autoscaling';
    } else if (targetPrefix === 'AnyScaleScalingPlannerFrontendService') {
      service = 'autoscaling-plans';
    }
  } else if (region == null && service.startsWith('s3-')) {
    region = service.slice(3).replace(/^fips-|^external-1/, '');
    service = 's3';
  } else if (service.endsWith('-fips')) {
    service = service.slice(0, -5);
  } else if (region && /-\d$/.test(service) && !/-\d$/.test(region)) {
[service, region] = [region, service];
  }
  return [HOST_SERVICES[service] || service, region]
}

export default {
  async fetch(request, env, ctx) {
    // Fill these in...
    const my_s3_domain = "s3.example.com";
    const my_bucket_name = "foundry"
    const my_account_id = "00000000000000000000000000000000";
    const my_access_key = "00000000000000000000000000000000";
    const my_secret_key = "0000000000000000000000000000000000000000000000000000000000000000";

    // Create the strings we need now
    const my_bucket_domain = my_bucket_name + "." + my_s3_domain;
    const cloudflare_s3_api = "https://" + my_account_id + ".r2.cloudflarestorage.com";
    const cloudflare_s3_bucket_api = "https://" + my_bucket_name + "." + my_account_id + ".r2.cloudflarestorage.com";

    // Make sure there is an Authorization header
    if (request.headers.get("authorization") === null) {
      return (new Response("Forbidden", { status: 403 }));
    }
    
    // Generate a new signature and change URL
    var s3_url;
    const request_url = new URL(request.url);

    if (request_url.hostname === my_s3_domain) {
      s3_url = cloudflare_s3_api;
    } else if (request_url.hostname === my_bucket_domain) {
      s3_url = cloudflare_s3_bucket_api;
      request_url.pathname = request_url.pathname.replace("worker/", "");
      s3_url = s3_url.concat(request_url.pathname, request_url.search)
    }
    
    const s3_request = new Request(s3_url, request);

    s3_request.headers.delete("accept-encoding");
    s3_request.headers.delete("cf-connecting-ip");
    s3_request.headers.delete("cf-ipcountry");
    s3_request.headers.delete("cf-ray");
    s3_request.headers.delete("cf-visitor");
    s3_request.headers.delete("x-forwarded-proto");
    s3_request.headers.delete("x-real-ip");

    const orig_signer = new AwsV4Signer({
      url: request_url,               // required, the AWS endpoint to sign
      accessKeyId: my_access_key,                                        // required, akin to AWS_ACCESS_KEY_ID
      secretAccessKey: my_secret_key,    // required, akin to AWS_SECRET_ACCESS_KEY
      sessionToken: undefined,       // akin to AWS_SESSION_TOKEN if using temp credentials
      method: request.method,             // if not supplied, will default to 'POST' if there's a body, otherwise 'GET'
      headers: s3_request.headers,            // standard JS object literal, or Headers instance
      body: request.body,               // optional, String or ArrayBuffer/ArrayBufferView – ie, remember to stringify your JSON
      signQuery: undefined,          // set to true to sign the query string instead of the Authorization header
      service: "s3",            // AWS service, by default parsed at fetch time
      region: "us-east-1",             // AWS region, by default parsed at fetch time
      cache: undefined,              // credential cache, defaults to `new Map()`
      datetime: request.headers.get("x-amz-date"),           // defaults to now, to override use the form '20150830T123600Z'
      appendSessionToken: undefined, // set to true to add X-Amz-Security-Token after signing, defaults to true for iot
      allHeaders: undefined,         // set to true to force all headers to be signed instead of the defaults
      singleEncode: undefined,       // set to true to only encode %2F once (usually only needed for testing)
    });

    // Make sure the original request is signed correctly
    if (request.headers.get("authorization") !== await orig_signer.authHeader()) {
      return (new Response("Invalid Authorization Signature", { status: 403 }));
    }

    const signer = new AwsV4Signer({
      url: s3_url,               // required, the AWS endpoint to sign
      accessKeyId: my_access_key,                                        // required, akin to AWS_ACCESS_KEY_ID
      secretAccessKey: my_secret_key,    // required, akin to AWS_SECRET_ACCESS_KEY
      sessionToken: undefined,       // akin to AWS_SESSION_TOKEN if using temp credentials
      method: s3_request.method,             // if not supplied, will default to 'POST' if there's a body, otherwise 'GET'
      headers: s3_request.headers,            // standard JS object literal, or Headers instance
      body: s3_request.body,               // optional, String or ArrayBuffer/ArrayBufferView – ie, remember to stringify your JSON
      signQuery: undefined,          // set to true to sign the query string instead of the Authorization header
      service: "s3",            // AWS service, by default parsed at fetch time
      region: "us-east-1",             // AWS region, by default parsed at fetch time
      cache: undefined,              // credential cache, defaults to `new Map()`
      datetime: s3_request.headers.get("x-amz-date"),           // defaults to now, to override use the form '20150830T123600Z'
      appendSessionToken: undefined, // set to true to add X-Amz-Security-Token after signing, defaults to true for iot
      allHeaders: undefined,         // set to true to force all headers to be signed instead of the defaults
      singleEncode: undefined,       // set to true to only encode %2F once (usually only needed for testing)
    });
    
    const auth_header = await signer.authHeader();
    s3_request.headers.set("Authorization", auth_header);
    
    return fetch(s3_request);
  },
};
```

## Setup URL Rewrites
We need to setup a simple URL rewrite now to redirect traffic that contains the correct HTTP headers to our worker/ path. We are doing this again to drsatically reduce the number of times our worker runs (100,000 a day on free tier). With these rules and the worker script above our worker will only run when starting up your FoundryVTT server, opening the file picker, changing directories, and creating directory / uploading a file. All public access will be directly to our R2 bucket.

Go to your website configuration page and then to the `Rules` tab, before selecting `Transform Rules`. Name it whatever you want, click edit expression, and put this expression in the box.
```
(http.host eq "foundry.s3.example.com" and http.request.uri.query contains "delimiter" and http.request.uri.query contains "prefix") or (http.host eq "foundry.s3.example.com" and http.request.uri.query eq "x-id=PutObject")
```
Then set the Path to `Rewrite to...`
`Dynamic | concat("/worker", http.request.uri.path)`

Then Deploy!

## Enjoy!