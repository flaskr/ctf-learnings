https://ethernaut.openzeppelin.com/
https://sepolia-faucet.pk910.de/

## Level 0:
- Follow the clues given by contract.info()
- Hints:
    - You will need to call methods whose names are not directly available.

Security takeaways:
- Storage variables are visible by public, typically exposed via a public getter `contract.<variableName>()`.
- Do not store private information in storage.

## Level 1: Fallback
- Takes careful reading of the contract.
- You will need all of the given hints.

Security takeaways:
- Receive functions can be called by sending ether - be careful what you put in there.
- Be very wary of who/what can change any permissioned `owner`s.

## Level 2: Fallout
- Using Remix actually helped

Security takeaways:
- Font can mislead you - try to pick a font where `I`, `l`, `1` look different if possible.
- Comments can also mislead you from the code if you're not careful.

## Level 3: Coin Flip
- Another smart contract in the same block can arrive at the same guess.

Security takeaways:
- Randomness on chain is tricky because EVM code execution is designed to be deterministic. Bytecode is public, block height and hash are also publicly available and can be computed in the same block. At the moment, randomness is best obtained using VDFs like those provided by Chainlink.

## Level 4: Telephone
- What are the values of `tx.origin` and `msg.sender` in contract B when called by contract A?

Security takeaways:
- Usage of `tx.origin` to verify access is not typically a good pattern. It can break compatability with smart contract wallets or allow malicious contracts to pretend to be your user when they interact with them for different purposes.

## Level 5: Token
- If you turn an odometer down from 0..

Security takeaways:
- Before solidity 0.8.0, integer values can overflow or underflow without errors in solidity. It was recommended to use a SafeMath library to check for these especially when user input is involved. Since solidity 0.8.0, uints will revert when they overflow or underflow, by default.

## Level 6: Delegation
- `msg.data` contains the encoded function call, including the function selector.

Extra tips:
- `chisel` from foundry is a great CLI tool for running bits of solidity code.
- This is how to obtain a function selector from a string that represents the function signature `bytes4(keccak256(bytes(<function_signature>)))`

Security takeaways:
- `delegatecall` allows execution of foreign code on the present contract's state. Examine any delegatecall carefully, paying attention to the call target, and data we pass to the target.

## Level 7: Force
- You do need another contract. 
- You also need something that was depcrecated in solidity in version 0.8.18.

Security takeaways:
- You cannot prevent someone from sending ether to a contract, do not rely on the contract's balance for critical functionality.

## Level 8: Vault
Extra tips:
- Contract storage slots can be read using `cast storage <contract address> <slot>`. (`cast` is part of foundry toolkit)
- It is also possible to use a tx tracer to determine the input data used in the contract constructor.

Security takeaways:
- Everything sent or stored on-chain can be read, including 'private' storage slots, or function calldata.

## Level 9: King
- A contract can be a king.
- What are the differences between `call`, `send` and `transfer`?

Security takeaways:
- `call` is the recommended way to transfer ETH. Be wary of the differences between the different methods, and use this as a code smell.

## Level 10: Re-Entrancy
- Reverts can be caught by other contracts.
- Recall that `transfer()` on a contract can trigger its `receive()` function

Security takeaways:
- When calling external contracts, rely on 'Checks-Effects-Interactions' pattern to prevent re-entrancy attacks.
- There are 'readonly reentracy' attacks across multiple contracts, too.

## Level 11: Elevator
- Set `contract.top` to `true`.
- The function modifer of 'isLastFloor' is not set to `view`, which means that the state can be changed.

Security takeaways:
- Restrict function access modifiers where possible. 

## Level 12: Privacy
- `chisel` is one of the tools you can use to see storage slots - e.g. `cast storage <address> <slot>` -> `cast storage 0x01 1`.
- static arrays are a lot more straightforward to read than dynanmic ones.

Security takeaways:
- All data posted to the public blockchain is public. Watch out for any assumptions of data privacy in the smart contracts.
