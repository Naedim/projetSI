# NOEL Damien L3CDA- SI
Développement d'un programme de modélisation graphique 
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
  + [Exercice 4.4 : Interpréteur de script V2](#exercice-4-4-intepr%C3%A9teur-de-script-v2)  
 + [Bilan critique :Avis et améliorations](#bilan-critique-avis-et-am%C3%A9liorations)
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

## Exercice 4-1 Réferencement des objets et enregistrement des commandes
Ajout des classes *References* et *Environment*

(Pas de difficultés rencontrées)

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
**Resultat lors des commandes**
![Exercice-4-1-Resultat](https://github.com/Naedim/projetSI/blob/master/ex4_1.gif)

## Exercice 4-2 Ajout et suppression dynamique d'éléments graphiques

Ajout des classes AddElement, DelElement, NewImage, NewString, NewElement
(Pas de difficultés rencontrées)

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
(difficultés rencontrées lors de la mise en place de la suppression récursive de GContainer. Résolution du problème en créant une classe ContentManagement dont hérite la classe Référence, cette classe permet de mémoriser et récupérer facilement les éléments contenus dans un GContainer, utile pour la suppression récursive). 


Classe ContentManagement : 

```java
public abstract class ContentManagement 
{
	String[] content = new String[0];
	
	public void addContent(String element)
	{
		content = Arrays.copyOf(content, content.length+1);
		content[content.length-1] = element;
	}
	
	public String[] getContent()
	{
		return this.content;
	}
}
```

Commandes exécutées :
```
-(space add robi (rect.class new))
-(space.robi setDim 300 300)
-(space.robi setColor yellow)
-(space.robi add ala (rect.class new))
-(space.robi.ala setDim 200 200)
-(space.robi.ala setColor white)
-(space.robi.ala add hello (label.class new \"Hello world\"))
-(space.robi.ala.hello translate 10 10)
-(space.robi.ala.hello setColor black)
-(space.robi.ala add pif (image.class new france.jpg))
-(space.robi.ala.pif translate 100 0)
-(space.robi.ala.pif del pif)
-(space del robi)
```

La différence dans les commandes par rapport à l'exercice précédent est que l'on supprime tous les élément en supprimant seulement robi

**Resultat lors de l'execution des commandes


![Exercice-4-3-Resultat](https://github.com/Naedim/projetSI/blob/master/ex4_3.gif)

## Exercice 4-4 Interpreteur de script v2

Mise en place de la possibilité d'exécution de scripts à paramètres

(J'ai eu des difficultés à comprendre le fonctionnement des scripts, je n'avais pas intégré le fait que ces scripts devaient fonctionner comme des fonctions communes. J'ai de plus bloqué sur la manière de coder ce fonctionnement. Malgré ces difficultés, mes scripts sont fonctionnels)

Pour cela il a fallut ajouter les classes : Addscript, DelScript et RunScript

Classe AddScript : 

```java
public class AddScript implements Command
{
	private Environment environment;


	public AddScript(Environment environment) 
	{
		this.environment = environment;
	}
	
	@Override
	public Expr run(Reference receiver, ExprList method) 
	{
		if(receiver.getScriptByName(method.get(2).toString())!=null)
		{
			System.out.println("Le script " + method.get(2).toString() + " est déjà utilisé");
			return null;
		}
		receiver.addScript(method.get(2).toString(), (ExprList)method.get(3));
		
		System.out.println("Ajout du script " + method.get(2).toString() + ": " + method.get(3).toString());
		return null;
	}
	
	
}
```

Classe DelScript : 

```java
public class DelScript implements Command
{
	
	Environment environment;
	
	public DelScript(Environment environment) 
	{
		this.environment = environment;
	}
	
	@Override
	public Expr run(Reference receiver, ExprList method) 
	{
		if(receiver.getScriptByName(method.get(2).toString())==null)
		{
			System.out.println("le script " + method.get(2).toString() + "n'existe pas dans "+ method.get(0).toString());
			return null;
		}
		receiver.delScriptbyName(method.get(2).toString());
		System.out.println("suppression du script " + method.get(2).toString());
		return null;
	}
}
```

Classe RunScript : 

la méthode Run de cetet classe permet l'exécution d'un script avec les paramètres utilisés lors de la demande d'exécution d'un script :

- Elle récupère une copie du script que l'on souhaite exécuter (si il existe). 
- Elle modifie le contenu de la copie en modifiant le libellé des paramètres par les valeurs entrées lors de l'appel du script(si le 	script est valide). 
- Pour chaque commande dans le script, elle envoie la commande à l'intepréteur, ainsi le fonctionnement du reste du programme n'est pas altéré car l'on exécute les commandes de la même manière quand dans les exercices précédents.

```java
public class RunScript implements Command
{
	public Environment environment;

	//Hashmap of the arguments
	public Map<String, String> args = new HashMap<String, String>();
	
	
	public RunScript(Environment environment) 
	{
		this.environment = environment;
	}
	
	
	private ExprList convertArgs(ExprList el)
	{
		//Pour chaque expression de el
		for(int i = 0; i< el.size(); i++)
		{
			//On récupère l'expression dans la liste d'expression
			Expr e = el.get(i);
			
			
			//Si l'expression se révèle être une liste d'expressions (contient plus qu'un élément), on rapelle la méthode 
			if(e instanceof ExprList)
			{
				//Cette expression est égale à l'appel de la methode changeMethod
				el.set(i, this.convertArgs((ExprList)e));
			}
			else
			{
				String s = new String("");
				
				//On vérifie si l'expression est une expression pointée
				String[] arrOfStr = e.toString().split("\\.");
				
				
				//Si l'expression est une expression pointée, on la convertit une listExpression contenant les 	expression séparées
				if(arrOfStr.length>1)
				{
					//Création de la nouvelle expression
					
					//On modifie les valeurs des arguments existants dans la liste chainée en regrdant dans le Hashmap d'arguments
					
					//On recrée une expression avec les valeurs des arguments
					
					String stock = "";
					
					//On vérifie si le premier élément est un argument
					stock = this.args.get(arrOfStr[0]);
					
					//Si il est un argument
					if(stock!=null)
					{
						//On ajoute sa valeur à la string s
						s += stock;
					}
					else
					{
						//Sinon a on ajoute l'expression de base à la string s
						s += arrOfStr[0].toString();
					}
					
					
					for(int j=1; j<arrOfStr.length; j++)
					{
						//On refait la mêm echose que au dessus sauf que nous ajoutons le caractère '.' à l'élement que nous concatenons pour recréer la liste chainée
						stock = this.args.get(arrOfStr[j]);
						if(stock!=null)
						{
							s += ("." + stock);
						}
						else
						{
							s += ("." + arrOfStr[j].toString());
						}
						
						
					}
					
					//On modifie la liste d'expression
					el.set(i,fromStringToExpr(s));
				}
				//Si ce n'est pas une expression pointée
				else
				{
					//On vérifie si celle-ci est un des arguments du script

					s = this.args.get(e.toString());
					
					//Si il l'est on change l'argument par sa valeur
					if(s!=null)
					{		
						e = fromStringToExpr(s);	
					}
					el.set(i,e);
				}
				
				
			}
			
			
		}
		return el;
	}
	
	//Permet de convertir une chaîne de caractère en expression
	private Expr fromStringToExpr(String s)
	{
		//LispParser permettant de convertir la string en expr
		LispParser parser = new LispParser(s);
		Expr e = null;
		try 
		{	//Conversion
			e = parser.parseExpr();
		} catch (Exception e1) 
		{
			e1.printStackTrace();
		}
		
		//Renvoie de l'expression
		return e;
		
	}
	@Override
	public Expr run(Reference receiver, ExprList method) 
	{
		//Iterator pour parcourir l'expression;
		
		
		ExprList script = receiver.getScriptByName(method.get(1).toString());
		if(script ==null)
		{
			System.out.println("Le script " + method.get(1).toString() + " n'existe pas" );
			return null;
		}
		
		//Itérateur pour récupérer les valeurs souhaitées 
		Iterator<Expr> it = script.iterator();
			
		//Si il n'y a rien dans le script
		if(!it.hasNext())
		{
			System.out.println("Le script est vide");
			return null;
		}

		// Liste des arguments entrés par l'utilisateur
		ExprList callArgs = new ExprList();
		
		//Ajout du receiver comme premier argument du script
		callArgs.add(method.get(0));
		
		//On récupère les autres arguments entrées lors de l'appel du script 
		for(int i=2; i<method.size(); i++)
		{
			callArgs.add(method.get(i));
		}
		
		//On récupère les arguments originaux du script
		ExprList scriptArgList = (ExprList) it.next();
		
		
		//Si le nombre d'argument donnée par l'utilisateur ne correspond pas au nombre d'arguments attendus par le script
		if(callArgs.size()!= scriptArgList.size())
		{
			System.out.println("Mauvais nombre d'argument lors de l'appel du script " + method.get(1).toString());
			return null;
		}
		
		
		//On met dans le hashmap les arguments
		for(int i = 0; i< callArgs.size(); i++)
		{
			this.args.put(scriptArgList.get(i).toString(), callArgs.get(i).toString());
		}
				
		//Si il n'y aucune commande dans le script
		if(!it.hasNext())
		{
			System.out.println("Le script n'as pas de commandes");
			return null;
		}
		
		
		//On récupère les commandes
		ExprList commandes = (ExprList) it.next();
		
		
		//On modifie les commandes du script en changeant les noms des arguments par leur valeur		
		
		commandes = this.convertArgs(commandes);
		
		for(Expr e : commandes)
		{
			new Interpreter().compute(environment, (ExprList)e);
		}
		
		return null;
	}
		
	
}
```
## Bilan critique :Avis et améliorations

Je suis plutôt satisait du travail effectué. Le programme me semble bien architecturé dans l'ensemble. 
Ce que je souhiaterai améliorer est la manière dont je gère la suppression récursive du contenu d'un GContainer. 
J'ai en effet géré l'implémentation  d'un GContainer dans un autre GContainer au sein de la classe Reference. Je trouve plus efficace, plus qualitatif d'attribuer un nouvel environnement à chaque nouveau GContainer, ce qui faciliterai la gestion du contenu d'un GContainer, notamment au niveau de la suppression récursive d'un GContainer.
