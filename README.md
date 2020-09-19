# My fork of Rattle

In this fork, I updated the intermediate representation (IR) of Rattle.
I decided to maintain some PUSH instructions that are related to PHI instructions.
This way, when symbolic executing, I know the exact location of PUSH instruction, and then, in which path it was declared.
Also, I added to SSAInstruction class a variable called instruction_offset.
This variable represents the byte-offset to the start of the range in the source file.
This will be helpful when trying to map one instruction to the Solidity source file.

# rattle

![Rattle](logo_s.png)

Rattle is an EVM binary static analysis framework designed to work on deployed smart contracts. Rattle takes EVM byte strings, uses a flow-sensitive analysis to recover the original control flow graph, lifts the control flow graph into an SSA/infinite register form, and optimizes the SSA – removing DUPs, SWAPs, PUSHs, and POPs. The conversion from a stack machine to SSA form removes 60%+ of all EVM instructions and presents a much friendlier interface to those who wish to read the smart contracts they’re interacting with.

## Example

```bash
$ python3 rattle-cli.py --input inputs/kingofether/KingOfTheEtherThrone.bin -O
```

Would produce a register machine output like this:

![King of Ether numberOfMonarchs](example.png)

Functions are recovered and split off. Additionally function arguments, memory locations, and storage locations are recovered.

## Usage
Rattle runs on the runtime contract hex string.

If you're running rattle on a contract you can compile with solidity, use the `--bin-runtime` option and strip off the header:
```console
$ solc --bin-runtime KingOfTheEtherThrone.sol 2>/dev/null | tail -n1 > contract.bin
```

## Dependencies

* python3
* graphviz
* cbor2
* pyevmasm

To install the python dependencies, run these commands:
```console
$ python3 -m venv venv
$ source venv/bin/activate
$ pip install -r requirements.txt
```

## Troubleshooting

If you get a syntax error like this:
```python
  File "rattle-cli.py", line 16
    def main() -> None:
               ^
SyntaxError: invalid syntax
```
You likely ran rattle with python2 instead of python3.

# Presentation

For more details on the Rattle design and features, see my reCON Montreal presentation, which is annotated [here](https://www.trailofbits.com/presentations/rattle/).

# License

Rattle is licensed and distributed under the AGPLv3 license. [Contact us](mailto:opensource@trailofbits.com) if you're looking for an exception to the terms.
