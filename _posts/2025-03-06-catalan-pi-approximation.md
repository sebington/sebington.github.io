# Pi et les nombres de Catalan

Depuis qu'les grands modèles de langage (LLM) sont rentrés dans nos vies là, quand il s'agit de tester un nouveau modèle, mon beau-frère et moi avons pour habitude de lui demander de générer un script en Python capable d'estimer la valeur du nombre Pi (3.141592653589793...). En général nous le laissons choisir la méthode utilisée et la plupart du temps le modèle nous propose la méthode dite "de Monte-Carlo" dans laquelle des points sont générés au hasard dans une zone carrée contenant un [quart de cercle](https://en.wikipedia.org/wiki/Approximations_of_%CF%80#/media/File:Pi_monte_carlo_en.gif). Le rapport entre le nombre de points tombés à l'intérieur du quart de cercle et le nombre de points tombés à l'extérieur donne une approximation de Pi. Au fil du temps, les LLM nous ont proposé d'autres méthodes pour approximer π et Dieu sait si elle sont nombreuses (https://fr.wikipedia.org/wiki/Approximation_de_%CF%80). Je possède un Jupyter Notebook qui recense mes nombreuses tentatives en conservant les résultats (réussites ou échecs). J'essaie de noter la date, le modèle utilisé, si l'inférence est locale ou en ligne, etc.

Avec l'arrivée récente de DeepSeek-R1 et des modèles qui "réfléchissent" avant de répondre, je me suis dit que je pourrais peut-être essayer une approche plus ambitieuse. Pourquoi ne pas demander au modèle de générer une approche nouvelle, jamais essayée auparavant ? C'est ce que j'ai fait avec le prompt suivant, très simple, soumis au modèle 4o de ChatGPT version gratuite en ayant pris soin d'activer la fonction "Reason" : "Find a new never-tried-before way to approximate the value of Pi".

Le modèle a réfléchi pendant 1 minute et 8 secondes et a généré la sortie suivante (désolé mais j'ai l'habitude de parler anglais avec ChatGPT).

![Logo](images/catalan.png)

Tout d'abord, j'ai été assez bluffé par le résultat, qui m'a semblé très original car faisant appel au [Nombre de Catalan](https://fr.wikipedia.org/wiki/Nombre_de_Catalan), un truc dont, en tant que non-mathématicien, je n'avais jamais entendu parler. En mathématiques, les nombres de Catalan "forment une suite d'entiers naturels utilisée dans divers problèmes de dénombrement". Surpris je l'ai été encore davantage quand j'ai fait quelques recherches sur Internet et constaté que cette méthode de calcul de la valeur de Pi semble assez rare. Si on fait une recherche, on trouve bien un rapport entre "Pi" et "Catalan", toutefois il ne s'agit pas dans le cadre du Nombre de Catalan mais dans celui de la [Constante de Catalan](https://fr.wikipedia.org/wiki/Constante_de_Catalan), les deux méthodes, même si elles sont attribuées à la même personne, n'ont apparemment rien à voir entre elles.

Au début je ne comprenais pas le lien entre les nombres de Catalan et Pi, c'est à dire entre la première et la deuxième équation.

Ce lien semble issu de la [formule de Stirling](https://fr.wikipedia.org/wiki/Formule_de_Stirling), qui "permet de calculer un équivalent asymptotique de la suite des nombres de Catalan". La seconde équation est donc équivalente à l'équation qui permet de calculer la suite des nombres de Catalan mais elle a le bon goût de faire appel à Pi. Comme on peut s'y attendre, ChatGPT a bien lu et digéré tout Wikipédia.

J'ai ensuite logiquement demandé à 4o de générer un script en Python pour tester son approche. Il s'agit donc dans un premier temps de calculer le nombre de Catalan pour n, puis d'injecter le résultat dans le dénominateur de l'équation permettant d'approximer Pi. Comme souvent avec les approximation numériques de Pi, plus n est grand, plus la valeur calculée de Pi est précise, avec un grand nombre de chiffres après la virgule qui sont en accord avec la "vraie" valeur de Pi.

Voici le code proposé par ChatGPT après un léger affinage :

```python
def catalan_number(n):
    """Calculate the n-th Catalan number."""
    from math import comb
    return comb(2 * n, n) // (n + 1)

def pi_approximation(n):
    """Approximate pi using the Catalan-Number Pi Approximation method for a single n."""
    C_n = catalan_number(n)
    return (16 ** n) / (n ** 3 * C_n ** 2)

# Choose the value of n for the approximation
n = 10
true_pi = 3.141592653589793  # Use a high-precision value for pi
approximation = pi_approximation(n)
error = abs(approximation - true_pi)

# Display the result
print(f"After {n} iterations, the calculated value of Pi is: {approximation}, with an error of {error}")
```

La première fonction ('catalan_number') calcule la valeur du nombre de Catalan et la seconde ('pi_approximation') la valeur approximative de Pi. Cette valeur est ensuite comparée à la valeur communément admise pour en déduire l'erreur. On peut changer la valeur de n pour affiner le résultat, au prix d'un temps de calcul légèrement plus long chaque fois qu'on ajoute un zéro. Pour arriver à 3,14, il faut environ n=1000, ensuite "the sky the limit".
