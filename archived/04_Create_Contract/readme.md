# WTF Starknet 4: Create Contract

In this chapter, we will explore the fundamental aspects of creating a smart contract from the ground up. We will begin by delving into the declaration of contract storage, defining a constructor function, and implementing both public and private functions. Furthermore, we will uncover the intricacies of creating and emitting events within our smart contract. By the end of this chapter, you will have a solid understanding of the core components involved in developing a smart contract.

## Storage

Every smart contract must have storage defined. This is done by using the `#[storage]` attribute followed by a special `struct` called `Storage`. Within this struct, you can define variables that are persistent and can be accessed or modified by the contract.

```rust
#[starknet::contract]
mod create_contract{
    #[storage]
    // Storage must be annoted by the #[storage] attribute
    struct Storage {
        // Define storage variables
        increment: u8,
    }
}
```

## Constructor

A constructor is a special type of function that runs only once when the contract is deployed. It can be used to initialize the state of the contract. In every contract, there can be only one constructor defined, and it is annotated with the `#[constructor]` attribute. The naming of the function can be anything, but as a best practice, the function should be named `fn constructor() {}`.

```rust
    #[constructor]
    // Constructor must be annotated by the #[constructor] attribute
    // As an input variable, we reference the state of the contract
    fn constructor(ref self: ContractState) {
        self.increment.write(0);
    }
```

More about construtor (here)[https://github.com/WTFAcademy/WTF-Cairo/tree/main/15_Constructor].

## Functions

Within a Starknet smart contract, there can be two types of functions: public functions and private functions. Public functions can be accessed externally by anyone, while private functions can only be accessed internally.

### Public Functions

Public functions are defined with the `#[external(v0)]` attribute, which indicates that all functions within the implementation can be accessed externally. The `v0` version indicates that the current contract remains compatible with future compilers.

```rust
    // Public Functions are defined with the #[external(v0)] attribute
    // #[generate_trait] will implicitly generate our interface.
    #[external(v0)]
    #[generate_trait]
    impl CreateContract of ICreateContract {

    }

```

#### Read-only Functions

Read-only functions are functions that cannot change or mutate the state of the contract; they can only read from it. The contract state is passed as a snapshot to these functions using `self: @ContractState`.

```rust
    // Read-only function
    // @ContractState indicates that we pass a snapshot of the ContractState
        fn get_increment(self: @ContractState) -> u8 {
            self.increment.read()
        }
```

More about snapshots (here)[https://github.com/WTFAcademy/WTF-Cairo/tree/main/22_Snapshot].

#### Read/Write Functions

Read/Write functions are functions that can change and mutate the state of the contract. The contract state is passed as a reference to these functions using `ref self: ContractState`.

```rust
        // Read/Write function
        // ref self: ContractState indicates that we pass the reference of the ContractState
        fn increase_increment(ref self: ContractState) {
            let current_increment = self.increment.read();

            if current_increment == 64 {
                // calling an internal function
                self.reset_increment();
            } else {
                self.increment.write(current_increment + 1);
            }

        }

```

More about references (here)[https://github.com/WTFAcademy/WTF-Cairo/tree/main/21_Reference].

### Private Functions

Private functions do not have the `#[external(v0)]` attribute, as seen below. This indicates that the functions within the implementation are private and can only be accessed within the contract.

```rust
    // Private Functions that doesn't have the #[external(v0)] attribute
    #[generate_trait]
    impl InternalFunctions of InternalFunctionsTrait {
        fn reset_increment(ref self: ContractState) {
            self.increment.write(0);
        }
    }
```

## Interfaces

Interfaces define the structure and behavior of the contract and serve as the contract's public ABI (Application Binary Interface). Interfaces are defined using the `#[starknet::interface]` attribute, which explicitly specifies the contract's interface. We have also seen the `#[generate_trait]` attribute, which implicitly defines the contract's interface.

```rust
// Defining an explicit interface
#[starknet::interface]
trait IExplicitCreateContract<TContractState> {
    fn set_increment(ref self: TContractState, value: u8);
}

#[starknet::contract]
mod create_contract{

    // ...

    // Public Function with an explicitly defined interface
    #[external(v0)]
    impl ExplicitCreateContract of super::IExplicitCreateContract<ContractState> {
        fn set_increment(ref self: ContractState, value: u8) {
            self.increment.write(value);
        }
    }
}
```

More about interfaces, (here)[https://github.com/WTFAcademy/WTF-Cairo/tree/main/25_Interface].

## Events

Events are data that are emitted by the contract and can be stored on Starknet. Events are defined using the `#[event] `attribute and must be defined as ` enum Event{}`.

```rust
    #[event]
    #[derive(Drop, starknet::Event)]
    // Events must be annoted by the #[event] attribute
    // In addition we need to derive the Drop and starknet::Event traits
    // Inside the Event enum, we define all created events
    enum Event {
        CounterIncreased: CounterIncreased,
    }

    #[derive(Drop, starknet::Event)]
    // Deriving the starknet::Event indicates that this struct will be emiting events
    struct CounterIncreased {
        amount: u8,
    }
```

We will emit the event within the `increase_increment` function, once we have incremented the value.

```rust
            // emiting an event
            self.emit(Event::CounterIncreased(CounterIncreased { amount: self.increment.read() }));
```

More about events, (here)[https://github.com/WTFAcademy/WTF-Cairo/tree/main/16_Events].

## Summary

In this chapter, we learned how to create a smart contract from scratch. We covered topics such as declaring our storage, defining a constructor function, and implementing both public and private functions. Additionally, we learned how to create events within our smart contract and how to emit them. You can find the deployed contract below:

Starkscan: link

Voyager: link
