# NOEL Damien L3CDA- SI
Développement d’un langage spécifique pour des animations graphiques simples 
*******************
+ [Exercice 1 : Prise en main du projet ](#exercice-1-prise-en-main-du-projet)
+ [Exercice 2 : Intepréteur de script V1](#exercice-2-intepr%C3%A9teur-de-script-v1)
  + [Exercice 2.1 : Script de configuration](#exercice-2-1-script-de-configuration)
  + [Exercice 2.2 : Script d'animation](#exercice-2-2-script-danimation)
+ [Exercice 3 : Mise en place des classes commandes](#exercice-3-mise-en-place-des-classes-commandes)
+ [Exercice 4 : Selection et execution des commandes](#exercice-4-selection-et-execution-des-commandes)
  + [Exercice 4.1 : Réferencement des objets et enregistrement des commandes](#exercice-4-1-réferencement-des-objets-et-enregistrement-des-commandes)
  + [Exercice 4.2 : Ajout et suppression dynamique d'éléments graphiques](#exercice-4-2-ajout-et-suppression-dynamique-déléments-graphiques)
  + [Exercice 4.3 : Ajouter des éléments à des conteneurs](#exercice-4-3-ajouter-des-éléments-à-des-conteneurs)
  + [Exercice 4.4 : Création et exécution de scripts](#exercice-4-4-création-et-exécution-de-scripts)
*******************
## Exercice 1 Prise en main du projet

**But de l'exercice:**
  * Déplacer robi jusqu'au bord droit
  * Déplacement jusqu'au bord bas
  * Déplacement jusqu'au bord gauche
  * Déplacement jusqu'au bord haut
  * Changement de couleur avec une couleur aléatoire

**Résultat avec redimmensionnement**

![prise-en-main-du-projet](https://github.com/Naedim/projetSI/blob/master/ex1.gif)

Aucune difficulté n'a été rencontré lors de cet exercice.
*******************
## Exercice 2 Intepréteur de script V1
Exécution d'un script sous forme d'expressions parenthèsées

(Pas de difficultés rencontrées)

## Exercice 2-1 Script d'initialisation de l'environnement 

```
- (script (space color black) (robi color yellow) )
```
**Resultat de l'execution du script**

![Exercice-2-1-Resultat](https://github.com/Naedim/projetSI/blob/master/ex2_1.JPG)

## Exercice 2-2 Script d'animation

Initialisation de l'environnement comme vu prédément

```
- (script (space color white) (robi color red) )
```

Animation de robi (script lancé dans une boucle)
```
- (script (robi translate 10 0) (space sleep 100) (robi translate 0 10) (space sleep 100) (robi translate -10 0) (space sleep 100) (robi translate 0 -10) (space sleep 100))
```
**Resultat de l'execution du script**

![Exercice-2-2-Resultat](https://github.com/Naedim/projetSI/blob/master/ex2_2.gif)

*******************
## Exercice 3 mise en place des classes commandes
Ici, on organise les actions à éffectuer lors de la lecture du script en les regroupant dans des classes "commandes..."

(Pas de difficultés rencontrées)

2 classes gérant le changement de couleur et de position de Robi :
  * RobiChangeColor
  * RobiTranslate
  
2 classes gérant le changement de couleur et la mise en pause du space:  
  * SpaceChangeColor
  * SpaceSleep
  
**Classe RobiChangeColor**
```java 
public class RobiChangeColor implements Command
{
	GRect robi;
	Color newColor;
	public RobiChangeColor(GRect robi, Color newColor) 
	{
	
		this.robi = robi;
		this.newColor = newColor;
	}
	
	@Override
	public void run()
	{
		this.robi.setColor(this.newColor);
	}
}
```

**Classe RobiTranslate**

```java
public class RobiTranslate implements Command
{
	GRect robi;
	Point p;
	
	public RobiTranslate(GRect robi, Point p) 
	{
	
		this.robi = robi;
		this.p = p;
	}
	@Override
	public void run() 
	{
		robi.translate(p);
		
	}

}

```

**Classe SpaceChangeColor**

```java
public class SpaceChangeColor implements Command 
{	
	GSpace space;
	Color newColor;
	public SpaceChangeColor(GSpace space, Color newColor) {
	
		this.space = space;
		this.newColor = newColor;
	}
	
	@Override
	public void run()
	{
		this.space.setColor(this.newColor);
	}
}
```

**Classe SpaceSleep**
```java
public class SpaceSleep  implements Command 
{
	int time;
	
	public SpaceSleep(int time) {
		
		this.time = time;
	}

	@Override
	public void run() 
	{
		Tools.sleep(this.time);
	}
}
```
**Exécution de script**

Initialisation de l'environnement comme vu prédément

```
- (script (space color white) (robi color red) )
```

Animation de robi (script lancé dans une boucle)
```
- (script (robi translate 10 0) (space sleep 100) (robi translate 0 10) (space sleep 100) (robi translate -10 0) (space sleep 100) (robi translate 0 -10) (space sleep 100))
```

**Resultat de l'exécution du script**

![Exercice-3-Resultat](https://github.com/Naedim/projetSI/blob/master/ex2_2.gif)
*******************
## Exercice 4 Selection et execution des commandes

Evolution de la structure du projet
(Pas de difficultés rencontrées)

## Exercice 4-1 Réferencement des objets et enregistrement des commandes
Ajout des classes *References* et *Environment*
* Reference : 
```java
public class Reference implements Expr
{
	HashMap<String, Command> commandList = new HashMap<String, Command>();
	
	Object receiver;
	
	
	public Reference(Object receiver)
	{
		this.receiver = receiver;
	}
	
	public Command getCommandeByName(String selector)
	{
		
		return this.commandList.get(selector);
		
	}
	
	public void addCommand(String selector, Command primitive)
	{
		this.commandList.put(selector, primitive);
	}
	
	public Expr run(ExprList method)
	{
		Command c = this.getCommandeByName(method.get(1).toString());
		c.run(this.receiver, method);
		return null;	
	}

	@Override
	public String getValue() {
		// TODO Auto-generated method stub
		return null;
	}
	

}
```

* Environment
```java
public class Environment 
{
	public Environment()
	{
	}
	private HashMap<String, Reference> referencesList = new HashMap<String, Reference>();
	
	public void addReference(String name, Reference r)
	{
		referencesList.put(name, r);
	}
	
	public Reference getReferenceByName(String name)
	{
		return(referencesList.get(name));		
	}
}
```

Commandes exécutées

```
- (space setColor red) 
- (robi translate 25 25) 
- (space sleep 2000)
- (robi setColor black)
```


## Exercice 4-2 Ajout et suppression dynamique d'éléments graphiques
Ajout des classes AddElement, DelElement, NewImage, NewString, NewElement

AddElement : 
```java
public class AddElement implements Command 
{

	Environment environment;
	
	public AddElement(Environment environment) 
	{
		this.environment = environment;
	}

	@Override
	public Expr run(Reference receiver, ExprList method)
    	{
		//Récupération du receiver
        	Object o = receiver.getReceiver();
        
        	//Si l'objet est bien 
        	if(o instanceof GSpace)
        	{
            		GSpace space = (GSpace) o;
            		Reference nr = (Reference) new Interpreter().compute(this.environment, (ExprList) method.get(3));
            
			Object o2 = nr.getReceiver();
            		if(o2 != null)
			{           	        	
            			if(o2 instanceof GElement)
                		{
                    			GElement e = (GElement) o2;
                    			environment.addReference(method.get(2).toString(), nr);
                    			space.addElement(e);
                    			space.repaint();
                		}
			}	
            
        	}
     
        	return receiver;
    	}

}
```
DelElement : 
```java
public class DelElement implements Command 
{
	private Environment environment;
	public DelElement(Environment environment) 
	{
		this.environment = environment;
	}

	@Override
	public Expr run(Reference reference, ExprList method) 
	{
		//Récupère l'élement dans lequel on souhaite supprimer des éléments
		Object o = reference.getReceiver();
		
		//Si l'objet est bien un GSpace(le seul élément dans lequel on peut supprimer d'autres éléments pour le moment)
        	if(o instanceof GSpace) 
        	{
        		//On convertit l'objet en GSpace
           		 GSpace space = (GSpace)o;
            
           		 //On récupère la référence de l'objet visé dans l'expression)
           		 Reference ref = this.environment.getReferenceByName(method.get(2).getValue());
            
            		//On récupère l'élément à supprimer
            		Object objet = ref.getReceiver();
            
            		//Si l'objet est bien un GElement
		    	if(objet instanceof GElement) 
		    	{
				//On le convertir en GElement
				GElement ge = (GElement)objet;
				//On supprime l'élément dans space
				space.removeElement(ge);
				//On supprime la référence de l'objet supprimé dans l'environnement
				this.environment.delReference(method.get(2).getValue());
				//On actualise la vue
				space.repaint();
            		}
        	}
        	//On retourne l'expression
		return method;
	}
}
```

NewImage : 
```java
public class NewImage implements Command 
{

	@Override
	public Expr run(Reference receiver, ExprList method) 
	{
		try
	    	{
			//Récupérationd de l'image dont le chemin a été donnée dans l'expression methode
			Image image = ImageIO.read(new File(method.get(2).toString()));

			//Création de l'élément GImage avec l'image récupérée
			GImage img = new GImage(image);

			//Création de la référence de la GImage
			Reference ref = new Reference(img);

			//Ajout de la possibilité d'utiliser la commande "translate" pourla GImage crée
			ref.addCommand("translate", (Command) new TranslateElement());
			return ref;
	    	}
	    	catch (IOException e)
	    	{
			e.printStackTrace();
	    	}
		
		return method;

   	 }

}
```
NewString :
```java
public class NewString implements Command 
{

	@Override
	public Expr run(Reference receiver, ExprList method) 
	{
		//Création de lz GString avec ce qu'a marqué l'utilisateur
	   	 GString s = new GString(method.get(2).getValue());
	    
	   	 //Créationd e la référence de la strin crée
		Reference ref = new Reference(s);
		
		//Ajout de la pssibilité d'utiliser les commandes "translate" et "setColor" pour la GString crée
		ref.addCommand("translate", (Command) new TranslateElement());
		ref.addCommand("setColor", new SetColor());
		return ref;
    	}
}

```
NewElement : 
```java
class NewElement implements Command 
{
	public Expr run(Reference reference, ExprList method) 
	{
		try {
			@SuppressWarnings("unchecked")
			GElement e = ((Class<GElement>) reference.getReceiver()).getDeclaredConstructor().newInstance();
			Reference ref = new Reference(e);
			ref.addCommand("setColor", new SetColor());
			ref.addCommand("translate", (Command) new TranslateElement());
			ref.addCommand("setDim", new SetDim());
			return ref;
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;
	}
}
```
Commandes d'exécution
```
- (space add robi (rect.class new))
-(robi translate 130 50)
-(robi setColor yellow)
-(space add momo (oval.class new))
-(momo setColor red)
-(momo translate 80 80)
-(space add pif (image.class new france.jpg))
-(pif translate 100 0)!" + 
-(space add hello (label.class new \"Hello world\"))
-(hello translate 10 10) 
-(hello setColor black)
-(space del pif)
-(space del momo)
-(space del robi)
-(space del hello)
```
![Exercice-4-2-Resultat](https://github.com/Naedim/projetSI/blob/master/ex4_2.gif)

## Exercice 4-3 Ajouter des éléments à des conteneurs

Modification du travail précédent pour nos GContainers puissent désormais contenir d'autres GContainers
**Resultat lors de l'execution du script**

![Exercice-4-3-Resultat](https://github.com/Naedim/projetSI/blob/master/ex4_3.gif)

## Exercice 4-4 Création et exécution de scripts

Dans cette partie il fallait pouvoir créer et enregistrer des scripts afin de pouvoir les ré-utiliser quand on le souhaite.

J'ai rencontré des difficultés pour l'execution des scripts.

Pour cela il a fallut ajouter plusieurs classes :
* AddScript
* DelScript

Ainsi qu'apporter des modifications dans la classe :
* Environment

