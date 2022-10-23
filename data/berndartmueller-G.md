## Table of Contents

- [[G-01] Tier removal check is not necessary in `JBTiered721DelegateStore.recordAddTiers`](#g-01-tier-removal-check-is-not-necessary-in-jbtiered721delegatestorerecordaddtiers)

### [G-01] Tier removal check is not necessary in `JBTiered721DelegateStore.recordAddTiers`

#### Description

A tier is considered removed if the bit in the `_isTierRemoved` bitmap is set accordingly. The function `JBTiered721DelegateStore.recordAddTiers` adds new tiers and sorts them by `contributionFloor`, no matter if a tier is removed or not. Therefore, the `_isTierRemoved` bitmap reads are unnecessary and can be safely removed.

#### Findings

[JBTiered721DelegateStore.sol#L724-L725](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L724-L725)\
[JBTiered721DelegateStore.sol#L717](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L717)

#### Recommended mitigation steps

Consider removing the `_isTierRemoved` bitmap reads.
