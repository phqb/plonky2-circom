## Setup

**Environment variables**

* `NODE_PATH`: patch to patched NodeJS
* `SNARKJS_PATH`: path to cloned snarkjs directory
* `RAPIDSNARK_PATH`: path to rapidsnark prover executable
* `POT_PATH`: path to `powersOfTau28_hez_final_25.ptau` file

## Generating proof ✅

**Command**

```
cargo test -r --color=always --package plonky2_circom_verifier --lib verifier::tests::test_recursive2_ed25519_verifier --no-fail-fast -- -Z unstable-options --show-output
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
...
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

## Generating zkey 0 ✅

**Command**

```
$NODE_PATH --trace-gc --trace-gc-ignore-scavenger --max-old-space-size=2048000 --initial-old-space-size=2048000 --no-global-gc-scheduling --no-incremental-marking --max-semi-space-size=1024 --initial-heap-size=2048000 --expose-gc $SNARKJS_PATH zkey new plonky2.r1cs $POT_PATH plonky2_0.zkey -v > zkey0.out
```

**View log**

```
tail -f ./zkey0.out
```

**Output**

* `plonky2_0.zkey`

## Contribute to phase 2 ceremony ✅

**Command**

```
$NODE_PATH --trace-gc --trace-gc-ignore-scavenger --max-old-space-size=2048000 --initial-old-space-size=2048000 --no-global-gc-scheduling --no-incremental-marking --max-semi-space-size=1024 --initial-heap-size=2048000 --expose-gc $SNARKJS_PATH zkey contribute -verbose plonky2_0.zkey plonky2.zkey -n="First phase2 contribution" -e="some random text" > contribute.out
```

**View log**

```
tail -f contribute.out
```

**Output**

* `plonky2.zkey`

## Exporting vkey ✅

**Command**

```
$NODE_PATH --trace-gc --trace-gc-ignore-scavenger --max-old-space-size=2048000 --initial-old-space-size=2048000 --no-global-gc-scheduling --no-incremental-marking --max-semi-space-size=1024 --initial-heap-size=2048000 --expose-gc $SNARKJS_PATH zkey export verificationkey plonky2.zkey verification_key.json -v
```

**Output**

* `./verification_key.json`

## Generating proof 

**Command**

```
$RAPIDSNARK_PATH plonky2.zkey witness.wtns proof.json public.json
```

**Output**

* `./proof.json`
* `./public.json`

## Verifying proof ✅

**Command**

```
${NODE_PATH} ${SNARKJS_PATH} groth16 verify verification_key.json public.json proof.json -v
```

**Output**

```
[INFO]  snarkJS: OK!
```