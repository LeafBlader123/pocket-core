# Pruning (Unsafe, Experimental)

**Note: The following information is intended for research, experimental, and transparency purposes only. It is not recommended for production use. Exercise caution and diligence when considering pruning for your needs.**

The IAVL store introduced pruning features in a pull request [cosmos/iavl](https://github.com/cosmos/iavl/pull/158/files). However, these pruning features were deemed unsafe for Pocket's requirements.

Given that the Pocket network is continuously growing, efforts were made to backport the pruning feature.

During the backporting process, it was discovered that an experimental branch called "Pruning" already existed in the original v0 repository. This branch was created by the original V0 developers and represented an experimental implementation of the pruning changes.

It is important to emphasize that this experimental pruning feature should **never be used for validator purposes**. Exercise caution, particularly in scenarios where your servicers can fallback to becoming validators.

If you are interested in contributing to the improvement of Pocket's pruning feature, please also consider reviewing [Persistence Replacement Bundle 1 #1467](https://github.com/pokt-network/pocket-core/pull/1467). This pull request aims to provide a safe and optimized storage layer for POKT by decoupling the IAVL store for Merkle tree generation, pruning it, and introducing a separate efficient store for application storage.

## Deleting TxIndexer.DB
The `TxIndexer.DB` file is the largest file once pruning is applied. While deleting it can save space, it is strongly advised against doing so due to safety concerns if you intend to validate. The TxIndexer enables more efficient queries and helps prevent replay attacks, making it essential for the network's security.

## Running the Entire Network in Pruned Mode
Running the network in pruned mode across all nodes is also strongly discouraged. It is crucial to maintain snapshots of full, non-pruned nodes as a backup in case of emergencies or chain halts.

## Snapshot Download
For those interested in obtaining pruned snapshots, you can download the following:

- [Download Snapshot Pruned 99800](https://www.youtube.com/watch?v=LLFhKaqnWwk)
- Size: ~700GB to 210GB (with txindexer)   **- reduced by approximately 70%**
- Size: ~700GB to 40GB (without txindexer) **- reduced by approximately 94%**

Please note that these sizes represent the reduction achieved through pruning.

## CLI
```sh
pocket utils unsafe-prune {up_to_block_height}
```

