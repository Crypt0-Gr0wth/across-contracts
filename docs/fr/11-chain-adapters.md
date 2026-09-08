# Chapitre 11 — Les chain adapters : l'abstraction des ponts canoniques par chaine

Le dossier `contracts/chain-adapters/` contient une implementation d'`AdapterInterface` par famille de chaine supportee : `Arbitrum_Adapter.sol`, `OP_Adapter.sol` (chaines OP Stack comme Optimism ou Base), `Polygon_Adapter.sol`, `Linea_Adapter.sol`, `ZkStack_Adapter.sol` (zkSync), `Solana_Adapter.sol`, et plusieurs variantes specialisees comme `Arbitrum_CustomGasToken_Adapter.sol` pour les chaines Arbitrum-Orbit dont le gas natif n'est pas ETH.

Chaque adapter encapsule les particularites du pont natif de sa chaine derriere les deux memes fonctions, `relayMessage` et `relayTokens` : par exemple, l'adapter Arbitrum doit gerer le systeme de retryable tickets et son mecanisme de frais en deux parties (soumission + execution), tandis que l'adapter OP Stack s'appuie sur le `CrossDomainMessenger` standard d'Optimism. Ces differences profondes d'implementation restent totalement invisibles pour le `HubPool`, qui se contente d'appeler `relayMessage`/`relayTokens` sur l'adapter enregistre pour chaque chaine sans connaitre les details du pont sous-jacent.

Le dossier contient egalement des « forwarders » (`Arbitrum_Forwarder.sol`, `Ovm_Forwarder.sol`, `ForwarderBase.sol`) utilises pour les chaines de deuxieme couche au-dessus d'une autre L2 (par exemple une chaine Arbitrum-Orbit elle-meme deployee au-dessus d'Arbitrum One), qui doivent relayer un message a travers deux ponts successifs plutot qu'un seul, ainsi que des « rescue adapters » (`Arbitrum_RescueAdapter.sol`, `Ethereum_RescueAdapter.sol`) dedies a la recuperation de fonds bloques dans des scenarios exceptionnels.

[Chapitre suivant : upgradeabilite UUPS et administration cross-chain du SpokePool](12-upgradeabilite.md)
