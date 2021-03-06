# fixparser

This is the ECMAScript framework for working with FIX protocol messages. Compliant with FIX 5.0 SP2.

The Financial Information eXchange (FIX) protocol is an electronic communications protocol initiated in 1992 for international real-time exchange of information related to the securities transactions and markets.

[![Build Status](https://travis-ci.org/logotype/fixparser.svg?branch=master)](https://travis-ci.org/logotype/fixparser) [![NPM Version](https://badge.fury.io/js/fixparser.svg)](http://badge.fury.io/js/fixparser) [![Code Climate](https://codeclimate.com/github/logotype/fixparser/badges/gpa.svg)](https://codeclimate.com/github/logotype/fixparser) [![Test Coverage](https://codeclimate.com/github/logotype/fixparser/badges/coverage.svg)](https://codeclimate.com/github/logotype/fixparser/coverage)


Quick start
-----------

Install with `npm install fixparser`.

Create an instance of the `FIXParser` class:

```javascript
import FIXParser from 'fixparser';
let fixParser = new FIXParser(),
    parsedMessage = fixParser.parse('8=FIX.4.2|9=51|35=0|34=703|49=ABC|52=20100130-10:53:40.830|56=XYZ|10=249|');
console.log(parsedMessage);
```

Features
--------
+ Fast, single-digit microsecond performance
+ Modern, written in ES6/ES2015/ES2016/ES2017
+ Validation (checksum and message length), includes FIX specification in parsed message
+ Supports various separators/start of headers (e.g. 0x01, ^ and |)
+ Clean and lightweight code
+ Supports both node.js and browser environments

Performance
-----------
```bash
┌─────────────────────────────────┬───────────────┬──────────────┬──────────────┐
│ FIX Messages                    │ Messages/sec  │ Microseconds │ Milliseconds │
│ 200,000 iterations (same msg)   │ 117,440 msg/s │ 8.5150 μs    │ 0.0085 ms    │
│ 200,000 iterations (same msg)   │ 125,078 msg/s │ 7.9950 μs    │ 0.0080 ms    │
│ 200,000 iterations (random msg) │ 67,137 msg/s  │ 14.8950 μs   │ 0.0149 ms    │
│ 200,000 iterations (same msg)   │ 110,742 msg/s │ 9.0300 μs    │ 0.0090 ms    │
│ 200,000 iterations (random msg) │ 69,420 msg/s  │ 14.4050 μs   │ 0.0144 ms    │
│ 200,000 iterations (same msg)   │ 113,701 msg/s │ 8.7950 μs    │ 0.0088 ms    │
│ 200,000 iterations (random msg) │ 68,611 msg/s  │ 14.5750 μs   │ 0.0146 ms    │
│ 200,000 iterations (same msg)   │ 110,865 msg/s │ 9.0200 μs    │ 0.0090 ms    │
│ 200,000 iterations (same msg)   │ 108,108 msg/s │ 9.2500 μs    │ 0.0092 ms    │
│ 200,000 iterations (same msg)   │ 107,585 msg/s │ 9.2950 μs    │ 0.0093 ms    │
└─────────────────────────────────┴───────────────┴──────────────┴──────────────┘
```
1.7 GHz Intel Core i7 (8 GB 1600 MHz DDR3), run with `npm run perf`.

Message format
--------------

The general format of a FIX message is a standard header followed by the message body fields and terminated with a standard trailer.

Each message is constructed of a stream of <tag>=<value> fields with a field delimiter between fields in the stream. Tags are of data type TagNum. All tags must have a value specified. Optional fields without values should simply not be specified in the FIX message. A Reject message is the appropriate response to a tag with no value.
Except where noted, fields within a message can be defined in any sequence (Relative position of a field within a message is inconsequential.) The exceptions to this rule are:

- General message format is composed of the standard header followed by the body followed by the standard trailer.
- The first three fields in the standard header are BeginString (tag #8) followed by BodyLength (tag #9) followed by MsgType (tag #35).
- The last field in the standard trailer is the CheckSum (tag #10).
- Fields within repeating data groups must be specified in the order that the fields are specified in the message definition within the FIX specification document. The NoXXX field where XXX is the field being counted specifies the number of repeating group instances that must immediately precede the repeating group contents.
- A tag number (field) should only appear in a message once. If it appears more than once in the message it should be considered an error with the specification document. The error should be pointed out to the FIX Global Technical Committee.

In addition, certain fields of the data type MultipleCharValue can contain multiple individual values separated by a space within the "value" portion of that field followed by a single "SOH" character (e.g. "18=2 9 C<SOH>" represents 3 individual values: '2', '9', and 'C'). Fields of the data type MultipleStringValue can contain multiple values that consists of string values separated by a space within the "value" portion of that field followed by a single "SOH" character (e.g. "277=AA I AJ<SOH>" represents 3 values: 'AA', 'I', 'AJ').

It is also possible for a field to be contained in both the clear text portion and the encrypted data sections of the same message. This is normally used for validation and verification. For example, sending the SenderCompID in the encrypted data section can be used as a rudimentary validation technique. In the cases where the clear text data differs from the encrypted data, the encrypted data should be considered more reliable. (A security warning should be generated).

Authors
-------

**Victor Norgren**

+ https://twitter.com/logotype
+ https://github.com/logotype
+ https://logotype.se


Copyright and license
---------------------

Copyright © 2017 logotype

Author: Victor Norgren

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to
deal in the Software without restriction, including without limitation the
rights to use, copy, modify, merge, publish, distribute, sublicense, and/or
sell copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:  The above copyright
notice and this permission notice shall be included in all copies or
substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
IN THE SOFTWARE.

--------------------------
Built with IntelliJ IDEA Open Source License

<a href="https://www.jetbrains.com/buy/opensource/"><img src="https://s3-ap-southeast-1.amazonaws.com/www.logotype.se/assets/logo-text.svg" width="200"></a>

The people at JetBrains supports the Open Source community by offering free licenses. Check out <a href="https://www.jetbrains.com/buy/opensource/">JetBrains Open Source</a> to apply for your project. Thank you!
