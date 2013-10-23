# Tournament [![Build Status](https://secure.travis-ci.org/clux/tournament.png)](http://travis-ci.org/clux/tournament) [![Dependency Status](https://david-dm.org/clux/tournament.png)](https://david-dm.org/clux/tournament)

Tournament provides a base class for stateful tournament types that used to be implemented in this repository.

All tournaments have a huge amount of common logic so the helper methods included on this base class is worth reading about even if you don't use this module directly.

You should read at least one of:

- [tournament base class and commonalities](./doc/base.md)
- [implementors guide](./doc/implementors.md)

Implementions:

- [duel](https://npmjs.org/package/duel)
- [ffa](https://npmjs.org/package/ffa)
- [groupstage](https://npmjs.org/package/groupstage)
- [masters](https://npmjs.org/package/masters)

## Example implementation usage
Create a new tournament instance from one of the separate implementations, then interact with helper functions to score and calculate results.

```js
var Duel = require('duel');
var d = new Duel(4, Duel.WB); // 4 players - single elimination

d.matches; // in playable order
[ { id: { s: 1, r: 1, m: 1 }, // semi 1
    p: [ 1, 4 ] },
  { id: { s: 1, r: 1, m: 2 }, // semi 2
    p: [ 3, 2 ] },
  { id: { s: 1, r: 2, m: 1 }, // grand final
    p: [ 0, 0 ] },
  { id: { s: 2, r: 1, m: 1 }, // bronze final
    p: [ 0, 0 ] } ]

// let's pretend we scored these individually in a more realistic manner
d.matches.forEach(function (m) {
  d.score(m.id, [1, 0]);
});

// now winners are propagated and map scores are recorded
d.matches;
[ { id: { s: 1, r: 1, m: 1 },
    p: [ 1, 4 ],
    m: [ 1, 0 ] },
  { id: { s: 1, r: 1, m: 2 },
    p: [ 3, 2 ],
    m: [ 1, 0 ] },
  { id: { s: 1, r: 2, m: 1 }, // 1 and 3 won their matches and play the final
    p: [ 1, 3 ],
    m: [ 1, 0 ] },
  { id: { s: 2, r: 1, m: 1 }, // 4 and 2 lost and play the bronze final
    p: [ 4, 2 ],
    m: [ 1, 0 ] } ]

// can view results at every stage of the tournament, here are the final ones
d.results();
[ { seed: 1, maps: 2, wins: 2, pos: 1 },
  { seed: 3, maps: 1, wins: 1, pos: 2 },
  { seed: 2, maps: 1, wins: 1, pos: 3 },
  { seed: 4, maps: 0, wins: 0, pos: 4 } ]
```

## Installation
For specific tournament usage, install the modules you want:

```bash
$ npm install duel ffa groupstage --save
```

To use these on in the browser, bundle it up with [browserify](https://npmjs.org/package/browserify)

```bash
$ npm dedupe
$ browserify -r duel -r ffa -r groupstage > bundle.js
```

## Running tests
Install development dependencies

```bash
$ npm install
```

Run the tests

```bash
$ npm test
```

## License
MIT-Licensed. See LICENSE file for details.
