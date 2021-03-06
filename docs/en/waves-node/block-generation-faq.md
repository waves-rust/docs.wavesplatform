# Block Generation FAQ

This article describes possible solutions for the issues that can happen during block generation.

## My node is running but it does not generate blocks

**Question**: My node is running fine (appending microblocks) and the block height signatures compared with [wavesexplorer.com](https://wavesexplorer.com/) are correct which indicates that the node is not on fork. Suddenly the node failed to generate blocks. The `waves.log` contains messages indicating that the node will try to generate block in xxx seconds (`Next attempt for acc=<your_node_address> in <xxx> seconds`) but the generating does not actually happen. What can I do?

**Answer**: Your node may fail to generate blocks because the block generation process is probabilistic. Such log messages (`Next attempt for acc=<your_node_address> in <xxx> seconds`) indicate that your node scheduled generating blocks according to [LPoS](/en/blockchain/glossary#lpos) formula after the last key block is received. However, there is always a chance that another node had scheduled to generate the block at earlier time. You can use [wavesexplorer.com](https://wavesexplorer.com/) website to check the actual generation timestamp and the ID of the node that succeeded (was the first one) to generate a block. The chances of a node to generate next block are proportional to the share of WAVES belonging or staked to the node.

**Note**: You can use [dev.pywaves.org](https://dev.pywaves.org/) website to review block generators performance ratio (Performance ratio = Generated blocks / Estimated blocks). Please keep in mind that short-term (daily) estimates are for reference only.

**Example**: At 11:59:20 your node planned an attempt to generate block at 12:00:05, but when you check at [wavesexplorer.com](https://wavesexplorer.com/), you see that another node generated block at 12:00:03.

You can find the records of your node generating attempts and check what node actually succeeded to generate a block in your `waves.log` file. If your node is installed from Deb package by default the log file is located in `/var/log/waves/` folder in other cases `${waves.directory}/logs/`. For more details about node logging, see [Logging Configuration](/en/waves-node/logging-configuration) article.

To find the records with generating attempts in `waves.log` file, execute the following grep command:

**Note**: By default the log files can be read only by the user running the node.

```bash
sudo grep "Next attempt\|Forging\| Block(" /var/log/waves/waves.log
```

**Response example with unsuccessful attempt**:

```bash
2020-09-07 10:27:55,076 DEBUG [appender-52] c.w.mining.MinerImpl - Next attempt for acc=3PMj3yGPBEa1Sx9X4TBAFeJCMMaE3wvKR4N in 508.710 seconds
2020-09-07 10:27:55,147 DEBUG [ecution-context-global-48] c.w.s.a.BlockAppender$ - [2ecdf572 134.209.30.86:56992] Appended Block(DeB2bwPeJkpDMpLEvHm8MRg3axpYtwupMZi5uRigrLZo,CPX3P6rvYttUhUFtM2MTHdJ4AALFyfdfDey5oH9CGJXP,3PJEPHsDNtfDRxxaja8wEp3mCXp5kpLYsLS,1599474474614,[])
2020-09-07 10:29:50,072 DEBUG [appender-52] c.w.mining.MinerImpl - Next attempt for acc=3PMj3yGPBEa1Sx9X4TBAFeJCMMaE3wvKR4N in 457.722 seconds
2020-09-07 10:29:50,161 DEBUG [ecution-context-global-48] c.w.s.a.BlockAppender$ - [7895562c 173.249.1.184:60940] Appended Block(2fXUoLdh4XmDkuTfdZ7bPyvubBfEdhQ989X5P27nbHby,8hs8fTy52sJyzJwxMb75A38JAxsEPjycMTyfCbbrW9XB,3PEFQiFMLm1gTVjPdfCErG8mTHRcH2ATaWa,1599474589929,[],600000000)
```

This example response indicates that the node `3PMj3yGPBEa1Sx9X4TBAFeJCMMaE3wvKR4N` received block `DeB2bwPeJkpDMpLEvHm8MRg3axpYtwupMZi5uRigrLZo` and at `2020-09-07 10:27:55,076` the node planned an attempt to generate the next block in `508.710` seconds (~ at 10:34:23). The response also says that the processing of block `DeB2bwPeJkpDMpLEvHm8MRg3axpYtwupMZi5uRigrLZo` is complete and then the node received the next validated block `2fXUoLdh4XmDkuTfdZ7bPyvubBfEdhQ989X5P27nbHby` generated by `3PEFQiFMLm1gTVjPdfCErG8mTHRcH2ATaWa` node at `2020-09-07 10:29:50,072` (earlier than the node `3PMj3yGPBEa1Sx9X4TBAFeJCMMaE3wvKR4N` planned generating the next block). In this case the node `3PMj3yGPBEa1Sx9X4TBAFeJCMMaE3wvKR4N` did not succeed to generate new block at 10:34:23 because the new block was generated earlier by another node `3PEFQiFMLm1gTVjPdfCErG8mTHRcH2ATaWa`.

**Response example with successful attempt**:

```bash
2020-09-06 04:06:13,517 DEBUG [appender-52] c.w.mining.MinerImpl - Next attempt for acc=3PMj3yGPBEa1Sx9X4TBAFeJCMMaE3wvKR4N in 41.305 seconds
2020-09-06 04:06:13,556 DEBUG [ecution-context-global-48] c.w.s.a.BlockAppender$ - [560c392d 5.189.182.6:52504] Appended Block(3bQwytTjwQCkQs2DWuoR5oqNKFtjAyDSftHQXrW2ALLQ29MpVBuX96231JW9joTGsYbbuyHaEuhrfUVvgFxdnJBs,2rTRaJqMrp2L3HvUfJ4FRQQGZGPM23kHVdhy1pAQucHLAvyG7QEHy6mMw9MfV7cjf7r2BDWYeyv7Eih3Uz83yVog,3P2HNUd5VUPLMQkJmctTPEeeHumiPN2GkTb,1599365173456,[],600000000)
2020-09-06 04:06:54,829 DEBUG [block-miner-35] c.w.mining.MinerImpl - Forging with <3PMj3yGPBEa1Sx9X4TBAFeJCMMaE3wvKR4N>, Time 41369 > Estimated Time 41361, balance 3485157657499, prev block 2qNW6zpp419atqZbstbwnAijUAmk55ggWiSvFLu6eDDSMuMciMta9f8aNXWh1HybQe2i2R2KwMcRrhHwd8by2Ya7 at 2228616 with target 61
2020-09-06 04:06:54,842 DEBUG [appender-52] c.w.mining.MinerImpl - Next attempt for acc=3PMj3yGPBEa1Sx9X4TBAFeJCMMaE3wvKR4N in 412.766 seconds
2020-09-06 04:06:54,887 DEBUG [block-miner-35] c.w.mining.MinerImpl - Forged and applied Block(5sWqTZbHP2p5T1ss31FnGx4s48an8Uf1czTbcHrkBwomRnN9j4HU9zw4Y7YvB1yWeHx3a17up3rF8397Upmbdntm,2qNW6zpp419atqZbstbwnAijUAmk55ggWiSvFLu6eDDSMuMciMta9f8aNXWh1HybQe2i2R2KwMcRrhHwd8by2Ya7,<3PMj3yGPBEa1Sx9X4TBAFeJCMMaE3wvKR4N>,1599365214825,[],2000000000) with cumulative score 525712542186004822512224
```

This example response indicates that at `2020-09-06 04:06:13,517` the node `3PMj3yGPBEa1Sx9X4TBAFeJCMMaE3wvKR4N` planned to generate block in `41.305` seconds and then in `41.305` seconds it started generating block and succeeded (`Forged and applied Block 5sWqTZbHP2p5T1ss31FnGx4s48an8Uf1czTbcHrkBwomRnN9j4HU9zw4Y7YvB1yWeHx3a17up3rF8397Upmbdntm`). In some cases, a block can be successfully generated by your node and that is indicated in the log, however that does not guarantee that the block will be accepted by the blockchain. There is a chance that another node generated new block simultaneously with yours and sent it to the blockchain faster. To avoid this, use fast internet connection and up-to-date hardware that suits node system requirements.
Use [wavesexplorer.com](https://wavesexplorer.com/) website to check block generation timestamps and the IDs of the nodes that succeeded to finally generate blocks that have been added to the blockchain.

**Note**: Failure to generate blocks may also be caused by problems with you wallet or node configuration. In this case the `Next attempt acc=...` messages will have wrong address (`acc=`).

## Other Issues

If you have other node-related issues to report or discuss with Waves community, see [Keep in Touch](/en/keep-in-touch/) chapter for contact information.
