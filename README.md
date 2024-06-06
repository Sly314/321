Finality Provider Information Registry
The bbn-test-4 testnet will focus on the security of the staked Bitcoins by testing the user's interaction with the BTC signet network. This will be a lock-only network without a Babylon chain operating, meaning that the only participants of this testnet will be finality providers and Bitcoin stakers. This effectively means that for the next testnet, finality providers will only be receiving Bitcoin Signet delegations and not have to vote for blocks.

Bitcoin holders that stake their Bitcoin can use Babylon's staking web application to select the finality provider they want to delegate their attestation of power to. They do so by including the finality provider's BTC public key in the self-custodial Bitcoin Staking script. Babylon will employ a Bitcoin indexer that collects all staking transactions and extracts the finality provider BTC public keys that receive delegations for display in the staking web application. While the BTC public key is the only identifying information required for a finality provider, it does not expose all the information that a finality provider might want to share to attract more stake delegations.

The Babylon web application will additionally employ the finality provider information registry in this repository to display additional information such as the finality provider's moniker, website, and identity. To protect this registry against spam, we require finality providers to submit a deposit using the self-custodial Bitcoin staking to lock 0.1 signet BTC for one year. The deposit will be fully in the custody of the finality provider, but not be counted as active stake and can be retrieved after the deposit period expires.

An entry can be created in this registry by opening a pull request containing:

Their identifying information combined with their BTC public key.
A signature of the information using the corresponding BTC private key.
A proof of submitting their deposit and the deposit having sufficient confirmations.
Finality providers can submit their information prior or after the testnet launch. To be included in the initial list that is displayed in the staking web app, they have to submit the information prior to the launch.

The rest of the document explains the steps to create your finality provider's keys and submitting the required information to be included in the registry.

1. Create Finality Provider Keys
Finality Provider BTC key generation is covered by steps 1-3 from this guide. These steps describe how to set up the EOTS manager and generate the finality provider keys using it. In this phase, finality providers should only use the EOTS manager to generate their BTC keys and sign their finality provider information (covered later in this guide). In later stages, finality providers will be expected to operate a live version of the EOTS manager in order to provide economic security to PoS chains.

At the end of these steps, your finality provider Bitcoin key pair will be generated. Make sure that you store the key pair or the mnemonic you have generated in a safe place, as it is going to be needed for your participation on PoS security in the future stages of the Babylon testnet. Finality providers that don't have access to their keys, will not be able to transition to later stages.

⚠ Store the mnemonic used for keys creation in a safe place.

2. Deposit self-lock BTC
Finality providers that want to register their information must make a deposit of 0.1 signet BTC using the self-custodial Bitcoin Staking script. This is required to keep the finality provider information registry open, but protect it from spam and entities that do not make a real commitment to the project. The deposit will be locked for 52560 blocks (i.e. ~one year), and will not be counted towards the active stake of the system. Note that the deposit is still fully in the custody of the creator of the transaction, but will only become unlocked after the deposit period expires.

⚠ Warning! The deposit amount of 10000000 signet satoshi is the minimum amount required for registration. Any deposit bellow this number will be considered an invalid registration. There is no need to deposit more than 10000000, but such deposits with higher value will be considered as a valid registrations.

The deposit is a Bitcoin transaction with an output containing the deposit value committing to the Babylon Bitcoin Staking script. A special set of values should be used for the deposit to be a valid one. More specifically, to create a valid deposit, you can follow the steps in this guide, with the following flags on the stakercli transaction create-phase1-staking-transaction command:

--finality-provider-pk=<fp_pk> The public key of your finality provider previous generated.
--staker-pk The public key of the account who funds the deposit transaction.
--staking-amount=10000000, i.e. 0.1 signet BTC
--staking-time=52560, i.e. ~1 year
--magic-bytes=62627434 "bbt4" as hex
--covenant-committee-pks=50929b74c1a04954b78b4b6035e97a5e078a5a0f28ec96d547bfee9ace803ac0
This public key does not have a discrete logarithm therefore rendering the unbonding and slashing paths of the Bitcoin Staking script unusable. This makes the timelock path the only usable path as it only requires the staker's key.
--covenant-quorum=1
--network=signet
