## Pi et les nombres de Catalan

Quand il s'agit de tester un nouveau grand modèle de langage (LLM), mon beau-frère et moi avons pour habitude de lui demander de générer un script en Python capable d'estimer la valeur du nombre Pi (3.141592653589793...). En général nous laissons le LLM se débrouiller quant à la manière de procéder et très souvent le modèle nous propose un script basé sur une méthode pseudo-aléatoire dite "de Monte-Carlo" dans laquelle des points sont générés au hasard sur une surface imaginaire en 2D. Il peut par exemple s'agir d'une zone carrée contenant un quart de cercle, comme sur [cette image animée](https://en.wikipedia.org/wiki/Approximations_of_%CF%80#/media/File:Pi_monte_carlo_en.gif).

Le rapport entre le nombre de points tombés à l'intérieur du cercle et le nombre de points tombés à l'extérieur donne une approximation de Pi. Au fil du temps, les LLM nous ont proposé d'autres méthodes pour approximer π et [Dieu sait si elles sont nombreuses](https://fr.wikipedia.org/wiki/Approximation_de_%CF%80). Je possède un Jupyter Notebook qui recense mes nombreuses tentatives et qui présente l'avantage de conserver les résultats (succès ou échecs). J'essaie de m'astreindre à noter la date, le modèle utilisé, et si l'inférence est locale ou en ligne.

Avec l'arrivée récente de [DeepSeek-R1](https://blog.octo.com/octo-comprendre-les-mecanismes-derriere-le-modele-deepseek-r1-(partie-1)) et des modèles qui "réfléchissent" avant de répondre, je me suis dit que je pourrais peut-être essayer une approche plus ambitieuse. Pourquoi ne pas demander au modèle de générer une méthode complètement nouvelle, jamais essayée auparavant ? C'est ce que j'ai fait avec le prompt suivant, très simple, soumis au modèle 4o de ChatGPT version gratuite, en ayant pris soin d'activer la fonction "Reason" (fonction qui semble avoir été récemment ajoutée pour suivre la mode, le modèle 4o ne fait en effet pas partie de la liste des [*reasoning models*](https://platform.openai.com/docs/models) comme o3-mini, o1-mini ou o1) : *Find a new never-tried-before way to approximate the value of Pi*.

Le modèle a réfléchi pendant 1 minute et 8 secondes et a généré une réponse très intéressante dont voici l'essence (NB : (1) j'ai l'habitude de converser avec ChatGPT en anglais et (2) j'ai fait une copie d'écran de la sortie du modèle car les pages Github ne permettent pas de reproduire les équations, qui sont au format LaTeX).

![ChatGPT output](/images/catalan.png)

Tout d'abord, j'ai été assez bluffé par le résultat (y compris par le rendu des équations), qui m'a semblé très original car faisant appel au [Nombre de Catalan](https://fr.wikipedia.org/wiki/Nombre_de_Catalan), un truc qu'en tant que non-mathématicien je n'avais jamais entendu parler. En mathématiques, les nombres de Catalan forment "une suite d'entiers naturels utilisée dans divers problèmes de dénombrement".

Surpris je l'ai été encore davantage quand j'ai fait quelques recherches sur Internet et constaté que cette méthode de calcul de la valeur de Pi semble assez rare. On trouve bien un rapport entre "Pi" et "Catalan", cependant il ne s'agit pas du Nombre de Catalan mais de la [Constante de Catalan](https://fr.wikipedia.org/wiki/Constante_de_Catalan), les deux méthodes, même si elles sont attribuées à la même personne, n'ont apparemment rien à voir entre elles.

Au début je ne comprenais pas le lien entre les nombres de Catalan et Pi, c'est à dire entre la première et la deuxième équation. Ce lien semble issu de la [formule de Stirling](https://fr.wikipedia.org/wiki/Nombre_de_Catalan#Propri%C3%A9t%C3%A9s_et_comportement_asymptotique), qui "permet de calculer un équivalent asymptotique de la suite des nombres de Catalan". Quand n est grand, la seconde équation présente un double avantage : elle constitue une approximation de l'équation des nombres de Catalan tout en faisant appel à Pi.

J'ai ensuite logiquement demandé à 4o de générer un script en Python pour tester son approche. Il s'agit donc dans un premier temps de calculer le nombre de Catalan pour n, puis d'injecter le résultat dans le dénominateur de l'équation permettant d'approximer Pi. Comme souvent avec les approximations numériques de Pi, plus n est grand, plus la valeur calculée est précise, avec davantage de chiffres après la virgule en accord avec la "vraie" valeur de Pi.

Voici le code proposé par ChatGPT après 2-3 itérations :

```python
def catalan_number(n):
    # Calculate the n-th Catalan number
    from math import comb
    return comb(2 * n, n) // (n + 1)

def pi_approximation(n):
    # Approximate pi
    C_n = catalan_number(n)
    return (16 ** n) / (n ** 3 * C_n ** 2)

# Choose the value of n for the approximation
n = 10
true_pi = 3.141592653589793  # Use a high-precision value for pi
approximation = pi_approximation(n)
error = abs(approximation - true_pi)

# Display the result
print(f"After {n} iterations, the calculated value of Pi is: \
{approximation}, with an error of {error}")
```

La première fonction `def catalan_number()` calcule la valeur du nombre de Catalan et la seconde `def pi_approximation()` la valeur approximative de Pi. Cette valeur est ensuite comparée à la valeur communément admise pour en déduire l'erreur. On peut changer la valeur de n pour affiner le résultat, au prix d'un temps de calcul légèrement plus long chaque fois qu'on ajoute un zéro. Pour arriver à 3.14 par exemple, il faut environ n=1000, ensuite "the sky the limit".

Malgré le peu d'infos trouvées sur Internet, je ne pense pas que cette méthode de calcul de la valeur de Pi soit aussi "originale" que je l'ai cru au départ. ChatGPT n'a pas créé du neuf *ex-nihilo* mais le modèle a su trouver un chemin peu emprunté dans les informations disponibles sur Internet. Je suis assez satisfait de cette petite trouvaille, mais j'ai du mal à en évaluer l'originalité. Si un "vrai" mathématicien passe par là, il ou elle pourra peut-être me dire ce qu'il en est. Ce qui est sûr, c'est qu'il y a de la beauté dans les mathématiques. Et qu'est-ce que l'IA, sinon la démonstration de cette beauté des mathématiques appliquées et de leur redoutable efficacité ?
