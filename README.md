# contract-safety-checklist

This checklist is a continuously evolving guide designed to assist developers working with Bitcoin Script and CashScript contracts. While it may not cover every possible scenario, it aims to provide a solid foundation for identifying and addressing common vulnerabilities and operational challenges. By following these guidelines, you can enhance the security and reliability of your contracts.

- Identify and list all entry points in the contract.
- Identify input and output restrictions, if any.
- Identify version check requirements (timelocks, etc.)
- Assess future upgrade vulnerabilities
  - Example(s):
    - Relying on commitment length, an attacker can update the nftCommitment in a way that creates a Denial of Service). Use: `require(tx.inputs[x].nftCommitment.length == 40);` or `require(tx.inputs[x].nftCommitment.length == <required_value>);`
- Determine who can inject inputs into the transaction, whether it's p2pkh or p2sh.
  - Example(s):
    - In cases where someone wants to store the PKH from an input into an NFTCommitment, they’ll probably split the lockingbytecode but if the input is from a p2sh then the stored value in the commitment will be incorrect leading to a DoS. `require(tx.inputs[x].lockingBytecode.length == 25);` or `require(tx.inputs[x].lockingBytecode.length == <required_value>);`
- Detect potential UTXO injections.
- Identify unintentional burns (FT/NFT)
- Identify unintentional minting
- Check for category leaks
  - Example(s): Tokens might be sent to unintended destinations.
- Check for NFT capability changes and ensure predictability
- Find deadlocks due to state updates
  - Example(s): 
    - A mutable NFT is converted to an immutable state, causing a limbo state for other contracts that still expect a mutable NFT.
    - Changes in NFT commitment fail to meet the checks of other contracts, rendering it unusable.
- Verify the Genesis configuration to prevent misconfigurations that could lead to failed states.
- Check for mathematical errors.
- Avoid split errors without checking the length
- Implement length validation for function arguments
- ??? [ Create a PR! ]
