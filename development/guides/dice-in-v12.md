---
title: Dice in v12+
description: The dice parser was changed in v12, adding new die types now needs to hook into it
published: true
date: 2024-08-09T11:10:21.933Z
tags: dice
editor: markdown
dateCreated: 2024-08-09T10:15:51.366Z
---

# Adding a new die type in v12
The die parser in v12 is changed, adding a new die type now involves hooking into it. This example adds an exploding die type that modifies the result rolled to reflect the total. The standard Foundry exploding die just keeps adding more dice to the end of the roll. This makes choosing the largest result impossible for the system to do. 

We will need three new files and a modification to the main js file.

1. A subclass of the parser.
2. A subclass of the rolls class.
3. A new die class.
4. modifications to the main js file to import these new classes and configure the system to use them.

## The parser

We need a class that extends foundry.dice.RollParser and replaces the `_onDiceTerm` method. This method will look for something that marks a dice expression as being custom. The new parser takes everything following the bare dice term (e.g. 1d6) and puts it in modifiers. We will look for a switch to turn on exploding dice there. The current Foundry implementation only allows alphanumeric symbols, but the parser also captures non-alphanumeric symbols and includes them in modifiers. Since the current dice parser does not use !, I chose this. /roll 1d6! will roll one exploding d6. This falls back to producing a normal die when it does not detect our switch.

```js
export class MySystemRollParser extends foundry.dice.RollParser {
  /**
   * Handle a dice term.
   * @param {NumericRollParseNode|ParentheticalRollParseNode|null} number  The number of dice.
   * @param {string|NumericRollParseNode|ParentheticalRollParseNode|null} faces  The number of die faces or a string
   *                                                                             denomination like "c" or "f".
   * @param {string|null} modifiers                                        The matched modifiers string.
   * @param {string|null} flavor                                           Associated flavor text.
   * @param {string} formula                                               The original matched text.
   * @returns {DiceRollParseNode}
   * @internal
   * @protected
   */
  _onDiceTerm(number, faces, modifiers, flavor, formula) {
    if (CONFIG.debug.rollParsing) {
      // eslint-disable-next-line no-console
      console.debug(
        this.constructor.formatDebug(
          'onDiceTerm',
          number,
          faces,
          modifiers,
          flavor,
          formula
        )
      );
    }

    // We're going to manipulate modifiers, make sure it's defined.
    const sanitisedModifiers = modifiers === null ? '' : modifiers;

    const loc = sanitisedModifiers?.indexOf('!');
    const useNewDieType = loc !== -1;

    if (useNewDieType) {
      // Remove the ! since it has been used to change the type of Die
      const alteredModifiers =
        sanitisedModifiers.slice(0, loc) + sanitisedModifiers.slice(loc + 1);

      return {
        class: 'MySystemDiceTerm',
        formula,
        modifiers: alteredModifiers,
        number,
        faces,
        evaluated: false,
        options: { flavor },
      };
    }

    return {
      class: 'DiceTerm',
      formula,
      modifiers: sanitisedModifiers,
      number,
      faces,
      evaluated: false,
      options: { flavor },
    };
  }
}
```
## The rolls class
The new rolls class looks for a node that has our new DiceTerm (MySystemDiceTerm) and if it finds it, instantiates a new die. If it does not find it, it instantiates a normal foundry die. Note that this adds a new original field, which captures the formula before any processing is done to it.

```js
import { MySystemDie } from './my-system-die.mjs';

export class MySystemRoll extends foundry.dice.Roll {
  /**
   * Instantiate the nodes in an AST sub-tree into RollTerm instances.
   * @param {RollParseNode} ast  The root of the AST sub-tree.
   * @returns {RollTerm[]}
   */
  static instantiateAST(ast) {
    return CONFIG.Dice.parser.flattenTree(ast).map((node) => {
      if (node.class === 'MySystemDiceTerm') {
        const { formula } = node;
        const dD = MySystemDie.fromParseNode(node);
        dD.original = formula;
        return dD;
      }

      const cls = foundry.dice.terms[node.class] ?? foundry.dice.terms.RollTerm;
      return cls.fromParseNode(node);
    });
  }
}
```

## The die class
The original field is returned as expression, this means that chat messages report the term that was entered. This class has a factory method to construct an instance, this is called by the roll class. I have commented out the line that invokes the standard `_roll`, since I neither want nor need physical dice nor manual input. If you want to use physical dice, you'll need to uncomment this and check for `(roll.result === this.faces)` instead of `(roll.result === undefined)`. Note that maximize doesn't make a lot of sense in this context.

```js
export class MySystemDie extends foundry.dice.terms.Die {
  original = '';

  /** @inheritdoc */
  get expression() {
    return this.original;
  }

  /* -------------------------------------------- */
  /*  Dice Term Methods                           */
  /* -------------------------------------------- */

  /**
   * Roll the DiceTerm by mapping a random uniform draw against the faces of the dice term.
   * @param {object} [options={}]                 Options which modify how a random result is produced
   * @param {boolean} [options.minimize=false]    Minimize the result, obtaining the smallest possible value.
   * @param {boolean} [options.maximize=false]    Maximize the result, obtaining the largest possible value.
   * @returns {Promise<DiceTermResult>}           The produced result
   */
  async roll({ minimize = false, maximize = false, ...options } = {}) {
    const roll = { result: undefined, active: true };
    // roll.result = await this._roll(options);

    if (minimize) {
      roll.result = Math.min(1, this.faces);
    } else if (maximize) {
      roll.result = this.faces;
    } else if (roll.result === undefined) {
      let result = this.randomFace();
      roll.result = result;

      while (result === this.faces) {
        result = this.randomFace();
        roll.result += result;
      }
    }

    this.results.push(roll);
    return roll;
  }

  /* -------------------------------------------- */
  /*  Factory Methods                             */
  /* -------------------------------------------- */

  /** @override */
  static fromParseNode(node) {
    let { number, faces } = node;

    if (!number) {
      number = 1;
    }

    if (number.class) {
      number = Roll.defaultImplementation.fromTerms(
        Roll.defaultImplementation.instantiateAST(number)
      );
    }

    if (faces.class) {
      faces = Roll.defaultImplementation.fromTerms(
        Roll.defaultImplementation.instantiateAST(faces)
      );
    }

    const modifiers = Array.from(
      (node.modifiers || '').matchAll(this.MODIFIER_REGEXP)
    ).map(([m]) => m);

    const cls = CONFIG.Dice.terms.l;

    const data = { ...node, number, faces, modifiers, class: cls.name };

    return this.fromData(data);
  }
}
```

## The main js class
You will need to hook these classes into the system. We can just completely replace `CONFIG.Dice.rolls` since our class falls back to the default behaviour.

```js
import { MySystemDie } from './dice/my-system-die.mjs';
import { MySystemRollParser } from './dice/my-system-parser.mjs';
import { MySystemRoll } from './dice/my-system-rolls.mjs';

…

  // Make the parser recognise and construct a MySystemDie
  CONFIG.Dice.parser = MySystemRollParser;
  CONFIG.Dice.rolls = [MySystemRoll];

  if (!('l' in CONFIG.Dice.terms)) {
    CONFIG.Dice.terms.l = MySystemDie;
  }
```




