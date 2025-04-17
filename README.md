# Secure Multiparty Computation (SMPC)

Stacked garbling, introduced by [Heath & Kolesnikov (2020)](https://eprint.iacr.org/2020/973) is an optimization of Yao's Garbled Circuits, an SMPC protocol.

To garble a circuit, the Generator creates a garbled truth table for each gate of the circuit, which the Generator transmits to the Evaluator.  With a traditional approach, if the circuit contains a branch, then the Generator transmits garblings for both branches.  This is a waste, as only one branch can be executed.

Stacked garbling eliminates this waste by combining the the material of both branches into a single bit stream that the Evaluator can decode.  Thus, the Generator only has to transmit bits equal to the length of the longest branch.

The [stacked_garbling notebook](https://github.com/RobertHolderIII/SMPC/blob/main/stacked_garbling.ipynb) demonstrates how this works at a conceptual level:  The single bit stream is an `XOR` of the encrypted bit streams of each branch.  The Generator encrypts each branch's bit stream with the label of the *other* branch.  The Evaluator receives the label for the active branch, which it uses to encrypt the inactive branch, as the Generator did.  The Evaluator `XOR`s that result with the combined bit stream to recover the encrypted bit stream for the active branch.