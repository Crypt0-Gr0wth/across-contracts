# Chapitre 1 — Presentation d'Across et de son architecture SpokePool/HubPool

Across est un protocole de transfert de tokens entre chaines fonde sur un modele « intents » plutot que sur un pont classique de type lock-and-mint : un utilisateur depose des tokens sur une chaine source, et un relayeur hors-chaine avance immediatement les tokens equivalents sur la chaine de destination, avant meme que le transfert cross-chain sous-jacent ne soit finalise. C'est ce qui permet a Across d'offrir des transferts quasi instantanes, contrairement aux ponts natifs dont la latence est bornee par le temps de finalite de la chaine source (parfois plusieurs minutes a plusieurs heures).

L'architecture repose sur deux familles de contrats : un `HubPool.sol` unique deploye sur Ethereum, qui sert de tresorerie centrale de liquidite et d'administrateur cross-chain de tout le systeme, et un `SpokePool.sol` deploye sur chaque chaine supportee (Arbitrum, Optimism, Base, Polygon, zkSync, Linea, etc.), qui recoit les depots des utilisateurs et les remplissages des relayeurs. Chaque `SpokePool` est un proxy UUPS dont l'admin est le `HubPool` lui-meme, ce qui donne au `HubPool` une « propriete cross-chain » sur l'ensemble des spoke pools.

Les tokens deposes sur la chaine source restent verrouilles dans le `SpokePool` d'origine jusqu'a ce qu'ils soient envoyes, par le pont canonique de la chaine, vers le `HubPool` sur Ethereum. Le relayeur qui a avance les fonds sur la chaine de destination est rembourse plus tard, une fois qu'un acteur hors-chaine appele le « dataworker » soumet une preuve merkle attestant que le remplissage a bien eu lieu.

Ce parcours s'appuie sur le depot clone a la date d'ecriture, branche `master`. Fichiers centraux : `contracts/spoke-pools/SpokePool.sol`, `contracts/hub-pool/HubPool.sol`, `contracts/libraries/MerkleLib.sol`, `contracts/chain-adapters/interfaces/AdapterInterface.sol`, `contracts/interfaces/V3SpokePoolInterface.sol`, `contracts/interfaces/HubPoolInterface.sol`.

Rien n'a ete installe, compile ni execute pour ecrire ces chapitres.

[Chapitre suivant : depositV3, le depot sur la chaine source](02-depositv3.md)
