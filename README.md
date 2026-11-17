# CS3SY26_ProjectProposal by Aline Teresa Mendoza from CS Unit of PSHS


## Project Proposal Template: 

### Project Title or Name of the Application	: The Potion Room
### Section			   	      : Cesium
### Group Members			    : BECHAYDA, Cleone Ferge ; FERRARIS, Jacob Christian ; ROQUE, Daniel Jan
### Date (Proposal Date)  : Nov 18, 2026

### Project Description:
The Potion Room is a game wherein you are playing an alchemist who lives off of selling potions. Main Game Mechanics are: The Brewing Process, the Counter (selling to customers), The Shop, The Garden (growing of ingredients), and the Alchemy Map.

#### The Garden:

#### The Alchemy Map:
A 2D board filled with hidden potion effects. It tracks where your potion marker is located and checks if your potion has obtained an effect. You pick and use ingredients to move your potion marker across the board toward target effects (effect tiles), but if you land on a boundary tile, your potion will be fully destabilized and you will have to restart.

#### The Brewing Process:
Represents harvested herbs and minerals. Each ingredient has a specific movement direction and can be ground down to travel further across the brewing map. Water is a key ingredient to this, as it will bring back your potion marker back to where it came from (depending on the water's numeric value).

#### The Counter: 
The Counter is where you sell the requested potions to the customer. If the potion doesn't meet the standards of the customer's requests, they do not pay you any gold and will leave. If you do fully meet their standards, they pay you in full.

#### The Shop: 
The Shop is where you buy ingredients that are out of stock, it only accepts gold. Each ingredient in the shop has a specific amount of stock until it is sold out and will be randomized every game week. 

### How the Game works:
At the start of each day, customers will line up at the counter and each customer has a request potion for you to brew. To brew a potion you shall access the brewing station wherein the ingredients in your inventory, the alchemy map, and the cauldron are present. Your goal is to reach the effect tile asked by the customer. The default x and y value of the potion marker on the map is (0, 0). If you add an ingredient, its stored x and y values are added to the current potion marker's coordinates. Adding further, you can grind these ingredients to further fluctuate the coordinate values. If you reach a boundary tile, the potion explodes and your potion marker will be set back to its default x and y coordinates. Your ingredients that have been used in the potion will not be returned. If you land on an effect tile, you can choose to sell this potion with the acquired tile to the customer, or continue moving the potion marker. Once sold, the customer evaluates if the potion they were asking is the exact same potion that they were given. Even if the customer is dissatisfied or not, they will leave and the next customer shall be asked for their request.





- Describes system components, features, and expected outputs clearly and logically. NOTE: Clearly described the elements(classes), their attributes and methods and how they will interact with each other.

- Include the description as well of your target demographics as the user of your application. 

- The description is then followed by the class diagram, depicting clearly what was described.

**Sample class Diagram from https://static.visual-paradigm.com/web/images/tutorials/class-diagram-tutorial/01-class-diagram-example.png:**

![Sample Diagram](https://static.visual-paradigm.com/web/images/tutorials/class-diagram-tutorial/01-class-diagram-example.png)

### Goals:

Describe the long term outcomes of your project followed by a couple of objectives to meet the goals stated in this paragraph

- Objective 1:BOOOM
- Objective 2:
- Objective n:

[Click here for Rubrics and Sample projects](https://docs.google.com/document/d/1xKd1CqK6DcNWS2_oJPBNtY5n3I3Hqiw8FY3W295Iqfk/edit?usp=sharing)
