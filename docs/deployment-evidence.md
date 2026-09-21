# Studio Dev Deployment Evidence

This document records the public evidence for the exact deployed source revision.

## Deployment

- Network: Studio Dev
- Chain ID: `61997`
- Deployer: actor7, `0x8581c4a532dd3f9b163b12809b1bd089f367147f`
- Contract: `0x40b8a22210420FEED97F3E54782d9Ee5f13afD3d`
- Deploy transaction: [`0x630c14a39d45ba8ea44b27658ad6ebb79aaf4db41762f32a11067b9819e6180c`](https://explorer-studio-dev.genlayer.com/tx/0x630c14a39d45ba8ea44b27658ad6ebb79aaf4db41762f32a11067b9819e6180c)
- Contract Explorer: [`0x40b8a22210420FEED97F3E54782d9Ee5f13afD3d`](https://explorer-studio-dev.genlayer.com/address/0x40b8a22210420FEED97F3E54782d9Ee5f13afD3d)
- Deployment status: finalized and accepted; the deployed interface contains 9 public methods (5 view, 4 write).

## Final authoritative readback

After the final v5 assessment and its replay:

| Field | Value |
| --- | --- |
| advisory state | `CLEARABLE` |
| advisory version | `5` |
| sealed | `true` |
| review count | `5` |
| final review index | `4` |
| advisory identity match | `true` |
| service-area match | `true` |
| rescission declared | `true` |
| sampling basis present | `true` |
| authority match | `true` |
| `can_clear_alert()` | `true` |
| `safe_to_clear_alert()` | `true` |

Final review explanation:

> DC Water's May 10, 2024 update explicitly rescinds the May 2024 Upper Northwest boil water advisory, references sampling/testing, originates from dcwater.com, and matches the described service area.

Final evidence digests:

```text
6a14d58038413a028cc85362d2f9c88d23ddc273defbfb37c34cc85af7493614
f14001a06886de8476a33f2aeea479eacafb4cf7f5d37565074e7f9943f814df
ecb7e13cba97be4fe163a374408b9f51d6ba371de3d79dd96cf62b8fff00a059
```

## Complete E2E matrix

Every successful write below reached finalized semantic execution and was followed by the relevant readback. Explorer links use the same contract and transaction hashes that were recorded by the Studio Next CLI.

| ID | Operation and expected outcome | Transaction |
| --- | --- | --- |
| E2E-01 | Register v1; state `REGISTERED`, version 1 | [`0x50352f0a3bbabbb57d691f67d4acf7d5cf51b49997189314a803d00c32560ffd`](https://explorer-studio-dev.genlayer.com/tx/0x50352f0a3bbabbb57d691f67d4acf7d5cf51b49997189314a803d00c32560ffd) |
| E2E-02 | actor8 seal attempt; `OWNER_ONLY`, state unchanged | [`0x45bb4f9247f27c30284794a8212f2e67049cb3058c7cc3c29758beda25fb0802`](https://explorer-studio-dev.genlayer.com/tx/0x45bb4f9247f27c30284794a8212f2e67049cb3058c7cc3c29758beda25fb0802) |
| E2E-03 | Owner seals v1; state `ACTIVE` | [`0x1820c13a9a70aecbaee12e6a27ec240389c742191be4d8de0c17fcf20888ccb4`](https://explorer-studio-dev.genlayer.com/tx/0x1820c13a9a70aecbaee12e6a27ec240389c742191be4d8de0c17fcf20888ccb4) |
| E2E-04 | Assess v1; unresolved result stored at index 0 | [`0x82aeac00b1132e183fa29eee16fea06629fc526a7d7526899524b697d67b4f24`](https://explorer-studio-dev.genlayer.com/tx/0x82aeac00b1132e183fa29eee16fea06629fc526a7d7526899524b697d67b4f24) |
| E2E-05 | Supersede v1 | [`0xdcb19cd16a465655a22781a3ba0e365c4878717401af28cb76b31c0d9239d57b`](https://explorer-studio-dev.genlayer.com/tx/0xdcb19cd16a465655a22781a3ba0e365c4878717401af28cb76b31c0d9239d57b) |
| E2E-06 | Register v2 | [`0xe6de396e64e7c6aaf24061c65460af4dfada61da4965e94d365bbb538b911250`](https://explorer-studio-dev.genlayer.com/tx/0xe6de396e64e7c6aaf24061c65460af4dfada61da4965e94d365bbb538b911250) |
| E2E-07 | Seal v2; state `ACTIVE` | [`0x7e3a96c62f390bd34798f52c6ff2c6541b8ac47bbdbcaa350baa665a25de6698`](https://explorer-studio-dev.genlayer.com/tx/0x7e3a96c62f390bd34798f52c6ff2c6541b8ac47bbdbcaa350baa665a25de6698) |
| E2E-08 | Assess v2; unresolved result stored at index 1 | [`0x822421e4b41aa9b2e0dc1fd4eb19b2c58911dd92b619ea3d71fd1124ef49ef49`](https://explorer-studio-dev.genlayer.com/tx/0x822421e4b41aa9b2e0dc1fd4eb19b2c58911dd92b619ea3d71fd1124ef49ef49) |
| E2E-09 | Supersede v2 | [`0xe9c8ab7932519de6d7f9b54f911f78504dc93c658fbf5e86b056bbe3f064a54a`](https://explorer-studio-dev.genlayer.com/tx/0xe9c8ab7932519de6d7f9b54f911f78504dc93c658fbf5e86b056bbe3f064a54a) |
| E2E-10 | Register v3 with a testing source | [`0xe5cca2ad685aaadcc91fa21d1d1c51608243a011bf117ac0ed882035851f0a21`](https://explorer-studio-dev.genlayer.com/tx/0xe5cca2ad685aaadcc91fa21d1d1c51608243a011bf117ac0ed882035851f0a21) |
| E2E-11 | Seal v3 | [`0x61819f18fa0589e48fcff3780ebbd848fd0f0ff28c4159f575d211ac28da83a2`](https://explorer-studio-dev.genlayer.com/tx/0x61819f18fa0589e48fcff3780ebbd848fd0f0ff28c4159f575d211ac28da83a2) |
| E2E-12 | Assess v3; `CLEARABLE`, index 2, all five predicates true | [`0xe724f28b681fed115ac64b827117b64d2cce21c787a8b7388f4ccab449f459ea`](https://explorer-studio-dev.genlayer.com/tx/0xe724f28b681fed115ac64b827117b64d2cce21c787a8b7388f4ccab449f459ea) |
| E2E-13 | Replay v3; returns index 2, count remains 3 | [`0x7fd04278aae3c2a48ffbc280729624561be6ea0144210b56770bff36006cf585`](https://explorer-studio-dev.genlayer.com/tx/0x7fd04278aae3c2a48ffbc280729624561be6ea0144210b56770bff36006cf585) |
| E2E-14 | Supersede v3 | [`0xff78b5cf178581db8047f14d9be0d5cc4597873bda389ac4812e0819a668b3db`](https://explorer-studio-dev.genlayer.com/tx/0xff78b5cf178581db8047f14d9be0d5cc4597873bda389ac4812e0819a668b3db) |
| E2E-15 | Invalid pure-numeric hash registration; `INVALID_ADVISORY_HASH` | [`0x3d8c8e39ba7d771d07724b992275ed5df50b85e2b4c662df626bab0c74b51988`](https://explorer-studio-dev.genlayer.com/tx/0x3d8c8e39ba7d771d07724b992275ed5df50b85e2b4c662df626bab0c74b51988) |
| E2E-16 | Register v4 with deliberately wrong area | [`0x97e2bd1639f2dfa5101c5941e278783ff5b8d872f26870bad9de123a21a18138`](https://explorer-studio-dev.genlayer.com/tx/0x97e2bd1639f2dfa5101c5941e278783ff5b8d872f26870bad9de123a21a18138) |
| E2E-17 | Seal v4 | [`0x50bfbf27031b3cbd967380f2b7eb154a7fbf0eef988a7589c400f5c6a3b0d6da`](https://explorer-studio-dev.genlayer.com/tx/0x50bfbf27031b3cbd967380f2b7eb154a7fbf0eef988a7589c400f5c6a3b0d6da) |
| E2E-18 | Wrong-area assess; `UNRESOLVED`, area match false, index 3 | [`0x997be343bcd3353978cf9512b4f5345572294615de07e987e532d8bdd7acc157`](https://explorer-studio-dev.genlayer.com/tx/0x997be343bcd3353978cf9512b4f5345572294615de07e987e532d8bdd7acc157) |
| E2E-19 | Supersede v4 | [`0x6cd406f7362dd3cc085f5f00b9c6c7c30bcd6439d4742e4c548647678c900cbf`](https://explorer-studio-dev.genlayer.com/tx/0x6cd406f7362dd3cc085f5f00b9c6c7c30bcd6439d4742e4c548647678c900cbf) |
| E2E-20 | Register final v5 | [`0x42178275435d0d4bb47b6d04adbddb2e8e0e7df90f9a3687b390552607e6f10f`](https://explorer-studio-dev.genlayer.com/tx/0x42178275435d0d4bb47b6d04adbddb2e8e0e7df90f9a3687b390552607e6f10f) |
| E2E-21 | Seal v5 | [`0x34b1f563b1065987c9099dee1c6023389a5b9cf7e6bd55a444e02d40688e8948`](https://explorer-studio-dev.genlayer.com/tx/0x34b1f563b1065987c9099dee1c6023389a5b9cf7e6bd55a444e02d40688e8948) |
| E2E-22 | Final v5 assess; `CLEARABLE`, index 4, consensus accepted | [`0x614bec7179e572ceea7530a8c1c9b4bd48835d5d2993abbad391bb3bcca25478`](https://explorer-studio-dev.genlayer.com/tx/0x614bec7179e572ceea7530a8c1c9b4bd48835d5d2993abbad391bb3bcca25478) |
| E2E-23 | Final replay; returns index 4, review count remains 5 | [`0x789e8c3cc25cdb0a7b2b02d5764b10fe5e9d9cb5918ac5233ea7f788bcbf1377`](https://explorer-studio-dev.genlayer.com/tx/0x789e8c3cc25cdb0a7b2b02d5764b10fe5e9d9cb5918ac5233ea7f788bcbf1377) |

## Public source evidence

- [May 8, 2024 DC Water advisory](https://www.dcwater.com/about-dc-water/media/news/drinking-water-advisory-dc-water-issues-boil-water-advisory-customers)
- [May 10, 2024 DC Water rescission](https://www.dcwater.com/about-dc-water/media/news/update-dc-water-lifts-boil-water-advisory-all-affected-customers)
- [DC Water sample-testing update](https://www.dcwater.com/about-dc-water/media/news/update-dc-water-repairs-broken-115-year-old-water-main-sample-testing)
