# On-chain Metadata Registry Standard for NFTs

## Preamble

| Field   | Details                                               |
|---------|-------------------------------------------------------|
| NMRSP   | <Unassigned>                                          |
| Title   | OnMeta - On-chain Metadata Registry for NFTs          |
| Author  | betafuzz                                              |
| Status  | Draft                                                 |
| Type    | Standard                                              |
| Created | 2023-10-10                                            |

## Abstract

Introducing OnMeta, an on-chain metadata registry standard for NFTs, aiming to supersede the prevailing off-chain metadata methods (like IPFS hashes) with a more streamlined, decentralized, and reliable metadata solution.

## Motivation

Current reliance on off-chain metadata, such as IPFS hashes, comes with challenges:

1. Dependence on external services, potentially risking metadata loss.
2. Absence of a unified standard causing inconsistencies.
3. Difficulties in verifying metadata authenticity.

OnMeta offers a transition to an on-chain metadata registry, ensuring metadata immutability, enhancing trust, and promoting interoperability.

## Specification

### Structure

The OnMeta reference format:
```
chain://contractAddress//tokenId
```
### Smart Contract Interface

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract OnMeta {
    mapping(uint256 => string) private _tokenURIs;

    function setMetadata(uint256 tokenId, string calldata metadata) external {
        _tokenURIs[tokenId] = metadata;
        emit MetadataSet(tokenId, metadata);
    }

    function getMetadata(uint256 tokenId) external view returns (string memory) {
        return _tokenURIs[tokenId];
    }

    event MetadataSet(uint256 indexed tokenId, string metadata);
}
```

### Example: Bored Ape Yacht Club (BAYC) Transition

Assuming BAYC employs a standard function like:

```solidity
function tokenURI(uint256 tokenId) public view virtual override returns (string memory) 
```

#### Steps:

1. **Deploy OnMeta**: Begin by deploying the OnMeta contract.

2. **Migration**:
   For each BAYC NFT intended for metadata migration on-chain:
   * Store the desired metadata using the OnMeta contract.
   * In the BAYC contract, update the reference by invoking:

```solidity
`setTokenURI(tokenId, "chain://[OnMetaContractAddress]");`
```

For a concrete example, if OnMeta's address is `0x1234567890abcdef...`, and updating metadata for token ID `42`, one would execute:

```solidity
`setTokenURI(42, "chain://0x1234567890abcdef...");`
```

By following this protocol, the BAYC contract will direct straight to the OnMeta contract for metadata retrieval. Platforms such as OpenSea will recognize this novel URI format, and extract the metadata from OnMeta using the supplied contract address and token ID.

## Rationale

Shifting to OnMeta promises:

1. **Permanence**: Blockchain-stored data remains immutable.
2. **Decentralization**: Metadata won't hinge on services like IPFS nodes.
3. **Interoperability**: A singular standard assures uniform metadata access.
4. **Trust**: Direct blockchain verification of metadata becomes feasible.

## Backwards Compatibility

OnMeta doesn't interfere with prevailing off-chain metadata techniques. It offers an alternative. Transitioning might necessitate marketplaces to revise their metadata retrieval methods, but the long-term trust and reliability benefits arguably justify the initial implementation commitment.

## Conclusion

OnMeta, the NFT Metadata Registry Standard Proposal, strives to advance the reliability and authenticity of NFT metadata by transitioning from off-chain to on-chain storage, ensuring metadata remains consistent, authentic, and permanently accessible. 
