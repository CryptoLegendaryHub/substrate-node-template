This is what just have been added

1. runtime/Cargo.toml

line 57 - line 75

/////////////////////////////////////////////////

sp-io = { default-features = false, version = '2.0.1' }

[dependencies.chainbridge]
default-features = false
git = ''
package = 'chainbridge'
tag = 'v1.0.0'

[dependencies.example]
default-features = false
git = ''
package = 'example-pallet'
tag = 'v1.0.0'

[dependencies.erc721]
default-features = false
git = ''
package = 'example-erc721'
tag = 'v1.0.0'

//////////////////////////////////////////////////

line 105 - line 108

/////////////////////////////////////////////////

'sp-io/std',
'chainbridge/std',
'erc721/std',
'example/std',

/////////////////////////////////////////////////


2. runtime/src/lib.rs

line 11
/////////////////////////////////////////////////
use sp_io::hashing::blake2_128;
/////////////////////////////////////////////////

line 264 - line 298

////////////////////////////////////////////////


parameter_types! {
    pub const ChainId: u8 = 1;
    pub const ProposalLifetime: BlockNumber = 1000;
}

impl chainbridge::Trait for Runtime {
	type Event = Event;
	type AdminOrigin = frame_system::EnsureRoot<Self::AccountId>;
	type Proposal = Call;
	type ChainId = ChainId;
	type ProposalLifetime = ProposalLifetime;
}

parameter_types! {
    pub HashId: chainbridge::ResourceId = chainbridge::derive_resource_id(1, &blake2_128(b"hash"));
    // Note: Chain ID is 0 indicating this is native to another chain
    pub NativeTokenId: chainbridge::ResourceId = chainbridge::derive_resource_id(0, &blake2_128(b"DAV"));

    pub NFTTokenId: chainbridge::ResourceId = chainbridge::derive_resource_id(1, &blake2_128(b"NFT"));
}

impl erc721::Trait for Runtime {
	type Event = Event;
	type Identifier = NFTTokenId;
}

impl example::Trait for Runtime {
	type Event = Event;
	type BridgeOrigin = chainbridge::EnsureBridge<Runtime>;
	type Currency = pallet_balances::Module<Runtime>;
	type HashId = HashId;
	type NativeTokenId = NativeTokenId;
	type Erc721Id = NFTTokenId;
}

////////////////////////////////////////////////////////////

line 323 - line 325

////////////////////////////////////////////////////////////

ChainBridge: chainbridge::{Module, Call, Storage, Event<T>},
Example: example::{Module, Call, Event<T>},
Erc721: erc721::{Module, Call, Storage, Event<T>},

////////////////////////////////////////////////////////////


