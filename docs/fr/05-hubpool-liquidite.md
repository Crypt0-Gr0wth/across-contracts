# Chapitre 5 — Le pool de liquidite du HubPool et le taux de change des parts LP

`addLiquidity` et `removeLiquidity` (`contracts/hub-pool/HubPool.sol`) permettent a n'importe qui de deposer ou retirer un token L1 dans le pool central du `HubPool`, en echange de parts LP fongibles frappees ou brulees par `LpTokenFactory.sol`. Ce pool de liquidite est ce qui finance a la fois les remplissages lents (chapitre 4) et les remboursements de relayeurs (chapitres 6 a 9) lorsque les fonds doivent transiter par Ethereum.

`exchangeRateCurrent` calcule le taux de change entre une part LP et le token sous-jacent selon la formule documentee dans le code : `(liquidReserves + utilizedReserves - undistributedLpFees) / lpTokenSupply`. `liquidReserves` represente les fonds immediatement disponibles dans le pool, `utilizedReserves` represente le solde net envoye vers les spoke pools (positif si le `HubPool` a envoye plus qu'il n'a recu, negatif dans le cas inverse), et `undistributedLpFees` represente les frais LP deja collectes mais pas encore integres au taux de change.

Ces frais ne sont pas distribues instantanement mais degages progressivement dans le temps via `lpFeeRatePerSecond` (fixe a `1500000000000`, soit une fraction infime par seconde) : `accumulatedFees := min(undistributedLpFees * lpFeeRatePerSecond * timeFromLastInteraction, undistributedLpFees)`. Ce lissage temporel evite qu'un gros depot de frais fasse sauter instantanement le taux de change et incite les fournisseurs de liquidite a rester investis plutot que de retirer juste apres l'encaissement d'un gros lot de frais.

La fonction `sync` reconcilie l'etat interne du contrat avec son solde reel de tokens, au cas ou des tokens auraient ete transferes directement au `HubPool` sans passer par `addLiquidity` (par exemple via un pont natif qui livre des fonds bridges depuis un spoke pool) — un cas explicitement gere par un ajustement de `utilizedReserves` et `liquidReserves`.

[Chapitre suivant : proposeRootBundle, la proposition bondee de racines merkle](06-root-bundle.md)
