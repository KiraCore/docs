# [⏎](README.md#Roadmap) KIP_23
> Upsert Code of Conduct (Proposal)

The Code of Conduct or SLA (Service-level agreement) is a text document (markdown) containing expected off-chain and on-chain behavior of all network actors.

By curation of the Code of Conduct (on-chain SLA) governance of Kira can transparently define its own rules, vision and processes used to effectively achieve consensus. Is the result users can trust the network and understand how to become a network actor such as a validator or governance member.

## Data Reference Registry

Code of Conduct will be part of the `Data Reference Registry` (DRR) mapping hash and external URL to the document or file containing relevant data. The DRR is a key-value store mapping a friendly key name to metadata such as the data hash, reference URL (such as IPFS link), encoding and size.

### Example Structure

```
{
    "CodeOfConduct": {
        "hash": <Blake2-string>,
        "reference": <URL-string>,
        "encoding": <string>,
        "size": <int64>
    },
    "FrontendRelease": { ... },
    "BackendRelease": { ... },
    ...
}
```

## Upsert Code of Conduct

By submitting `upsert_drr` a proposal should be created that will enable governance set to update **CodeofConduct** record. The upsert DRR function should have an assigned identifier in the functions registry.

_INFO: It must be possible to define execution cost in genesis  - [Execution Fee](/spec/fees.md) to submit `upsert_drr` proposal._

Proposal should have following properties:
* Configurable in genesis `expiration` time (uint32) - seconds since submission
* Configurable in genesis `enactment` time (uint32) - seconds since expiration
* Allowed vote types: `yes`, `no`, `veto`, `abstain`
* JSON object containing key to which changes should apply and the relevant value such as hash, ref, encoding and size.
* Status with following types:
  * `undefined` - 0x00
  * `active` - 0x01
  * `rejected` - 0x02
  * `passed` - 0x03
  * `enacted` - 0x04

_NOTE: We should pre-configure in genesis (testnet) that Network Actors with assumed roles `governance` and `validator` should by default posses ability to propose and vote on `upsert_drr`._

## Proposal Vote

By submitting `vote` transaction along proposal identifier it should be possible for the governance members with the whitelisted permission to vote on `upsert_drr` to submit relevant vote types to pass or reject action of upserting Code of Conduct or other data reference change.

