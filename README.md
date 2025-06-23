SmartChat (based on Smartian)
========

Smartian is a grey-box fuzzer for Ethereum smart contracts. Smartian leverages
static and dynamic data-flow analyses to enhance the effectiveness of fuzzing.
Also, the SmartChat extension to Smartian provides new options for fuzzing from LLM-generated 
initial seeds. There is also a vulnerability-guided seed selection mechanism used for 
selecting seeds based on previously detected bugs in the seed pool. 

# Installation

SmarrChat is written in F#, so you have to install .NET to run it.
Installation step differs for each Linux distribution, so please refer to this
[link](https://docs.microsoft.com/en-us/dotnet/core/install/) and install
net5.0. Then, you can simply clone and build Smartian as follows.

```
$ git clone https://github.com/PAMunb/SmartChat
$ cd SmartChat
$ make
```

# Usage

You can fuzz a smart contract with Smartian by providing its EVM bytecode and
ABI specification as follows. Here, `-t` option specifies the time limitation in
seconds. The output test cases and bug-triggering inputs will be stored in the
directory specified by `-o` option.

```
$ dotnet build/Smartian.dll fuzz -p <bytecode file> -a <abi file> -t <time limit> -o <output dir>
```

The output directory will have two subdirectories. First, `testcase` directory
will contain inputs that increased edge coverage during fuzzing. You can use
these inputs to measure code coverage achievement. Second, `bug` directory will
contain inputs that triggered bug. The file names of bug-triggering inputs will
be tagged with abbreviated bug class name (e.g., 'RE' for reentrancy bug).  For
the list of used abbreviations, please refer to our paper.

Note that the generated test inputs are in JSON format, and they contain
the necessary information required to reproduce the transactions. You can replay
these files against the target contract with the following command.

```
$ dotnet build/Smartian.dll replay -p <bytecode file> -i <test case directory>
```
You may also check other command-line options of Smartian by running `dotnet
build/Smartian.dll fuzz --help` and `dotnet build/Smartian.dll replay --help`.


# Original Smartian Citation

You can site our paper with the following bibtex entry. The page field will be
updated later.
```bibtex
@INPROCEEDINGS{choi:ase:2021,
  author = {Jaeseung Choi and Doyeon Kim and Soomin Kim and Gustavo Grieco and Alex Groce and Sang Kil Cha},
  title = {{Smartian}: Enhancing Smart Contract Fuzzing with Static and Dynamic Data-Flow Analyses},
  booktitle = {Proceedings of the International Conference on Automated Software Engineering},
  year = 2021
}
```
