
## Segmentation_using_Tensor
# 1. Introduction
Une image est un moyen de transférer des informations, et l'image contient beaucoup d'informations utiles. Comprendre l'image et extraire des informations de l'image pour accomplir certains travaux est un important domaine d’application de la technologie de l’image numérique et la première étape dans la compréhension de l'image est la segmentation de l'image. En pratique, il n’est souvent pas intéressé par tous certaines parties de l'image, mais uniquement pour certaines zones qui ont les mêmes caractéristiques. La segmentation d'image est l'un des points chauds du traitement d'image et vision par ordinateur. C'est aussi une base importante pour l'image reconnaissance. Il est basé sur certains critères pour l'image d'entrée dans un certain nombre de la même nature de la catégorie afin d'extraire la zone où les gens sont intéressé. Et il est la base de l'analyse d'image et compréhension de l'extraction et de la reconnaissance des caractéristiques de l'image italicized text
 

# . augmentation du retour d'une image
Le code suivant effectue une simple augmentation du retour d'une image. De plus, l'image est normalisée à [0,1]. Enfin, comme mentionné ci-dessus, les pixels du masque de segmentation sont étiquetés {1, 2, 3}. Par souci de commodité, soustrayons 1 du masque de segmentation, ce qui donne des étiquettes qui sont: {0, 1, 2}.


# .Définir le modèle
Le modèle utilisé ici est un U-Net modifié. Un U-Net se compose d'un encodeur (sous-échantillonneur) et d'un décodeur (sur-échantillonneur). Afin d'apprendre des fonctionnalités robustes et de réduire le nombre de paramètres pouvant être entraînés, un modèle pré-formé peut être utilisé comme encodeur. Ainsi, l'encodeur pour cette tâche sera un modèle MobileNetV2 pré-formé, dont les sorties intermédiaires seront utilisées, et le décodeur sera le bloc de suréchantillonnage déjà implémenté dans les exemples TensorFlow du didacticiel Pix2pix .

La raison de la sortie de trois canaux est qu'il existe trois étiquettes possibles pour chaque pixel. Considérez cela comme une multi-classification où chaque pixel est classé en trois classes.

# .Former le modèle
Maintenant, tout ce qui reste à faire est de compiler et de former le modèle. La perte utilisée ici est losses.SparseCategoricalCrossentropy(from_logits=True). La raison d'utiliser cette fonction de perte est que le réseau essaie d'affecter à chaque pixel une étiquette, tout comme la prédiction multi-classe. Dans le vrai masque de segmentation, chaque pixel a soit un {0,1,2}. Le réseau émet ici trois canaux. Essentiellement, chaque canal essaie d'apprendre à prédire une classe et losses.SparseCategoricalCrossentropy(from_logits=True)constitue la perte recommandée pour un tel scénario. En utilisant la sortie du réseau, l'étiquette attribuée au pixel est le canal avec la valeur la plus élevée. C'est ce que fait la fonction create_mask.

# .Faire des prédictions
Faisons quelques prédictions. Dans un souci de gain de temps, le nombre d'époques a été réduit, mais vous pouvez le régler plus haut pour obtenir des résultats plus précis.
