## Setup

**Environment variables**

* `NODE_PATH`: patch to patched NodeJS
* `SNARKJS_PATH`: path to cloned snarkjs directory
* `RAPIDSNARK_PATH`: path to rapidsnark prover executable
* `POT_PATH`: path to `powersOfTau28_hez_final_25.ptau` file

## Generating proof ✅

**Command**

```
cargo test -r --color=always --package plonky2_circom_verifier --lib verifier::tests::test_recursive_semaphore_proof --no-fail-fast -- -Z unstable-options --show-output
```

**Output**

* `../../test/data/proof.json`
* `../../circuits/constants.circom`

## Compiling circom ✅

**Command**

```
circom ../../circuits/plonky2.circom --r1cs --sym --c
```

**Output**

```
template instances: 999
non-linear constraints: 33833018
linear constraints: 0
public inputs: 540
public outputs: 0
private inputs: 16581
private outputs: 0
wires: 33566967
labels: 71653827
Written successfully: ./plonky2.r1cs
Written successfully: ./plonky2.sym
Written successfully: ./plonky2_cpp/plonky2.cpp and ./plonky2_cpp/plonky2.dat
Written successfully: ./plonky2_cpp/main.cpp, circom.hpp, calcwit.hpp, calcwit.cpp, fr.hpp, fr.cpp, fr.asm and Makefile
```

## Generating witness ✅

**Command**

```
./plonky2_cpp/plonky2 ../test/data/proof.json ./witness.wtns
```

**Output**

* `./witness.wtns`