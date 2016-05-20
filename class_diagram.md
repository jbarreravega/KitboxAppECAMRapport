# Diagramme de classe

La structure de notre logiciel peut se diviser en trois parties:

* La partie principale concerne l'organisation logique des classes qui
  permet de représenter le problème posé. Elle se compose des classes
  Piece, Color, PriceSupplier, Supplier, Cabinet, Block, Furniture.
* La seconde partie concerne la gestion des commandes et se compose des
  class OrderWizard, Order, OrderDetails, Status, OrderSupplier,
  OrderClient.
* Enfin la dernière partie fait le lien entre les classes du programme
  et leur stockage en base de donnée. Ces classes sont DBManager,
  OrderClientManager, OrderSupplierManager, StockManager.

La classe Program est la classe principale de notre application. Elle se
charge de lancer les différentes classes liées à l'interface graphique
tel que OrderStep1, OrderStep2 et d'autres.

## La partie principale

### Piece, Color, PriceSupplier, Supplier

Les classes Piece, Color, PriceSupplier, Supplier permettent de
représenter l'information que l'on possède et dont on a besoin concernant
une piece qui se trouve dans le stock de l'entreprise. Les pièces du
stock sont représentées par un identifiant unique, leur code. Il peut y
avoir plusieurs même pièces dans le stock. C'est pourquoi il est possible
de stocker la quantité de chaque pièce. Une instance de la classe Piece
représente une certaine quantité (quantity) de ce type de pièce.  Dans
cette instance on peut aussi accéder au nombre de pièces encore en stock
(stock_quantity). Par exemple si l'on veut trois panneaux arrières dont
le code est XYZ nous aurons une seule instance de la classe Piece qui
contient comme quantité 3 et comme code XYZ, cependant l'attribut
stock_quantity correspondra au nombre de totale de pièces présente dans le
stock.

Les couleurs (ou matières) sont stockées à part dans une classe séparée
afin de pouvoir faire facilement des recherches selon ce critère et afin
de ne pas devoir réencoder les couleurs pour chaque pièce mais de
pouvoir les choisirs dans une liste.

L'entité Supplier est séparée de l'entitée PriceSupplier car l'on
considère qu'un même Supplier peut fournir différentes pièces et qu'on
ne souhaite pas dupliquer l'information concernant ce fournisseur en
base de donnée.

Les meubles sont représentés par la classe Furniture, cette classe
contient plusieurs Blocks. Cette classe Block est une interface qui doit
être étendue et qui permet d'ajouter des blocs de différentes natures par
la suite afin que le programme puisse évoluer. L'implémentation de cette
classe Block qui est créée est la classe Cabinet qui représente un
casier.

### OrderWizard, Order, OrderDetails, Status, OrderSupplier et OrderClient

Les classes OrderWizard, Order, OrderDetails, Status, OrderSupplier et
OrderClient permettent de stocké l'information des commandes.  Les
commandes sont stockées de manière simple. Elles ne contiennent qu'une liste
des pièces commandées. La structure du meuble est donc perdue lorsque la
commande est enregistrée. Il n'y a pas moyen de retrouver dans le
logiciel les structures Furniture, Cabinet, Piece qui étaient créées par
l'utilisateur avant l'enregistrement de la commande. Les commandes sont
enregistrées comme étant une liste de pièces qui n'ont pas de liens entre
elles. Il est important que le logiciel vérifie bien la validité d'un
meuble lorsqu'il le transforme en commande afin qu'il n'y ait pas
d'erreur. Les commandes sont sauvegardées afin d'en garder un suivi et
surtout de pouvoir prédire les stocks nécessaires.

La classe OrderWizard s'occupe de faire la liaison entre la liste des
meubles (Furniture) que le client a composé et la création de sa
commande qui n'est qu'une liste des pièces nécessaires.

### DBManager, OrderClientManager, OrderSupplierManager, StockManager

Les classes DBManager, OrderClientManager, OrderSupplierManager,
StockManager s'occupent de sauvegarder l'information présente dans les
autres classes en base de donnée. Elles s'occupent aussi de restituer ce qui
est stocké en base de donnée en classe utilisable par le logiciel. Il y
a un manager général qui est instancié une fois au démarage du logiciel
et qui contient des managers spécifiques pour traiter chaque type de classe.

Chaque manager contient un lot de méthodes standards qui lui permet
d'écrire et de lire les objets dans la base de donnée. Mais chaque
manager peut contenir des méthodes spécifiques notamment des filtres qui
permettent de récupérer certains objets selon des critères spécifiques.

Ces managers peuvent fonctionner de deux manières différentes. La
première est que lors de leur instanciation il charge la totalité des
objets présents dans la base de donnée en mémoire comme un attribut de
la classe. Certaines méthodes de tris peuvent alors fonctionner en
parcourant cette liste en mémoire ce qui peut simplifier certains tris.
Cependant il est aussi possible au méthode de filtre d'exécuter
directement des recherches en SQL dans la base de donnée. Ceci à
l'avantage d'être bien plus puissant et rapide mais peut compliquer son
implémentation car une fois les données récupérées en base de donnée, il
faut encore les traduire en un objet compréhensible par le programme. Le
programmeur doit gérer et surveiller lui même quand il est nécessaires
d'enregistrer les données modifiées en mémoire dans base de donnée. En
effet une modification des propriétés d'un objet n'impliquent pas tout de
suite des changements dans la base de donnée. Il faut pour cela passer
par la méthode Save des managers.

