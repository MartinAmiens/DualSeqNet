Pour évaluer la génération d'une séquence d'acides aminés, il a été choisi de produire un score basé sur les critères et observtaion suivants:

## La protéine est-elle fonctionnelle ?

1) Composition en Acides Aminés :

Distribution Naturelle : Les 20 acides aminés ne sont pas également représentés dans les protéines naturelles. Certains (comme la Leucine, l'Alanine, la Glycine, la Sérine) sont très courants, tandis que d'autres (comme le Tryptophane, la Cystéine, l'Histidine) sont plus rares. Une séquence aléatoire aura souvent une distribution trop uniforme ou trop bizarre.

2) Présence d'Acides Aminés Clés :

* Cystéine (C) : Essentielle pour les ponts disulfures qui stabilisent de nombreuses structures 3D. Une absence totale ou une présence isolée rendrait la formation de ces ponts impossible.

* Proline (P) et Glycine (G) : La Proline introduit des coudes rigides, la Glycine apporte une flexibilité extrême. Leur placement est crucial. Une séquence aléatoire les placera n'importe où, rendant le repliement difficile.

3) Balance Hydrophobe/Hydrophile :

Les protéines globulaires (la plupart des protéines fonctionnelles) ont généralement un cœur hydrophobe (acides aminés "qui n'aiment pas l'eau") et une surface hydrophile (qui "aiment l'eau"). Une séquence aléatoire aura souvent des blocs de résidus hydrophobes en surface, ou un mélange chaotique, favorisant l'agrégation plutôt que le repliement correct.

4) Absence de Répétitions Extrêmes et Simples :

Les séquences hautement répétitives comme AAAAA (poly-alanine) ou PLPLPL sont rares dans les protéines globulaires fonctionnelles. Elles sont plutôt caractéristiques de régions de faible complexité ou de protéines structurales fibreuses. Une séquence aléatoire pure est susceptible de contenir de longues répétitions par pur hasard, ce qui est un signe d'une mauvaise "conception" protéique pour une fonction classique.

5) Potentiel d'Agrégation Faible :

De nombreuses séquences aléatoires contiennent des "patches" (zones) d'acides aminés très hydrophobes ou avec des charges non compensées qui, une fois exposées à l'eau, ont tendance à s'agglutiner avec d'autres protéines, formant des agrégats insolubles (comme du "grumeau"). Une "bonne" protéine a évolué pour éviter ou masquer ces zones.

6) Propension à la Structure Secondaire :

Les acides aminés ont des préférences pour former des hélices alpha, des feuillets bêta ou des boucles. Une "bonne" séquence protéique a des régions avec une propension forte et cohérente à former ces structures, qui sont les "briques" du repliement 3D. Une séquence aléatoire aura une propension très hétérogène et chaotique, rendant la formation d'une structure stable improbable.

7) Absence de Codons Stop Internes (si issue d'une traduction complète) :

Si tu génères la séquence à partir d'un ADN, elle doit commencer par une Méthionine (M) et ne doit pas contenir de codons stop internes (TAA, TAG, TGA) qui arrêteraient la traduction prématurément.