# CS3SY26_ProjectProposal by Aline Teresa Mendoza from CS Unit of PSHS


## Project Proposal Template: 

### Project Title or Name of the Application	: The Potion Room
### Section			   	      : Cesium
### Group Members			    : BECHAYDA, Cleone Ferge ; FERRARIS, Jacob Christian ; ROQUE, Daniel Jan
### Date (Proposal Date)  : Nov 18, 2026

### Project Description:
The Potion Room is a game wherein you are playing an alchemist who lives off of selling potions. Main Game Mechanics are: The Brewing Process, the Counter (selling to customers), The Shop, The Garden (growing of ingredients), and the Alchemy Map.

### Target Demographics:
 - Primary Audience: Casual gamers usually from ages 13-30 who enjoy cozy games, light crafting, resource management, Idle Games, and want a simpler introduction to Potion Craft.
 - Secondary Audience: Computer Science Enthusiasts who would like to peer-review the algorithm & other features and/or newbies like high school students.
 - User Needs: functional algorithm and game sequence, predictable movement rules, and replay-ability from the weekly resetting shop

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

### Class Specifications:
#### 1. GameManager (Controller)
Coordinates the game loop, day/week progression, and communication between subsystems.
##### Attributes:
- currentDay: int
- currentWeek: int
- player: Player
- alchemyMap: AlchemyMap
- cauldron: Cauldron
- garden: Garden
- shop: Shop
- counter: Counter
##### Methods:
+ startGame(): void
+ startNewDay(): void (advances garden growth, generates daily customers, checks if currentDay % 7 == 1 to restock shop)
+ endDay(): void
+ handleBrewAction(ingredient: Ingredient, grindLevel: int): void
+ handleSellAction(): void

#### 2. Player
Represents the alchemist's resources and inventory.
##### Attributes:
- name: String
- gold: int
- inventory: Map<Ingredient, Integer>
- brewedPotions: List<Potion>
##### Methods:
+ addGold(amount: int): void
+ spendGold(amount: int): boolean
+ addIngredient(item: Ingredient, qty: int): void
+ useIngredient(item: Ingredient): boolean
+ getInventory(): Map<Ingredient, Integer>

#### 3. Ingredient (Abstract Base Class)
Represents raw alchemy materials that shift the potion marker on the map.
##### Attributes:
 name: String
 deltaX: int
 deltaY: int
 basePrice: int
 grindMultiplier: int
##### Methods:
+ getName(): String
+ getBasePrice(): int
+ abstract calculateMovement(grindLevel: int): Vector2D

#### 4. Herb & Mineral (Subclasses of Ingredient)
Herb Attributes: - freshness: int
Herb Methods: + calculateMovement(grindLevel: int): Vector2D (multiplies deltaX and deltaY by 1 + grindLevel)
Mineral Attributes: - hardness: int
Mineral Methods: + calculateMovement(grindLevel: int): Vector2D (requires higher grind threshold; applies grindMultiplier)

#### 5. Water (Subclass of Ingredient)
Pulls the potion marker back toward (0, 0) proportionally to its numeric strength.
##### Attributes:
- dilutionValue: int
##### Methods:
+ calculateMovement(grindLevel: int): Vector2D (returns a vector moving toward (0, 0) capped by dilutionValue)

#### 6. AlchemyMap & Tile Hierarchy
Manages the 2D board, hidden effects, and hazard boundaries.
##### AlchemyMap Attributes:
- width: int
- height: int
- grid: Tile[][]
- markerX: int (default 0)
- markerY: int (default 0)
##### AlchemyMap Methods:
+ moveMarker(dx: int, dy: int): Tile
+ resetMarker(): void (sets markerX = 0, markerY = 0)
+ getTileAt(x: int, y: int): Tile
+ getMarkerPosition(): Vector2D

> Tile (Base Class):
 x: int,  y: int, isDiscovered: boolean
+ onLand(cauldron: Cauldron, map: AlchemyMap): String

> EffectTile (Subclass of Tile):
- effectName: String
+ onLand(cauldron: Cauldron, map: AlchemyMap): String (reveals tile and attaches effectName to the active potion)

> BoundaryTile (Subclass of Tile):
+ onLand(cauldron: Cauldron, map: AlchemyMap): String (destabilizes potion, clears cauldron, calls map.resetMarker())

#### 7. Cauldron & Potion
Handles active brewing state and produces finished potions.
##### Cauldron Attributes:
- usedIngredients: List<Ingredient>
- acquiredEffect: String
- isStable: boolean
##### Cauldron Methods:
+ addIngredient(item: Ingredient, grindLevel: int, map: AlchemyMap): void
+ bottlePotion(): Potion
+ clearCauldron(): void (resets ingredients without returning them to player inventory)
##### Potion Attributes:
- effectName: String
- sellValue: int
##### Potion Methods:
+ getEffectName(): String
+ getSellValue(): int

#### 8. Garden & GardenPlot
Allows players to grow renewable ingredients across in-game days.
##### Garden Attributes:
- plots: List<GardenPlot>
##### Garden Methods:
+ plantSeed(plotIndex: int, seedType: Ingredient): boolean
+ waterPlot(plotIndex: int): void
+ advanceDayGrowth(): void
+ harvestPlot(plotIndex: int, player: Player): boolean
##### GardenPlot Attributes:
- plantedIngredient: Ingredient
- daysGrown: int
- daysToMature: int
- isWatered: boolean
##### GardenPlot Methods:
+ grow(): void
+ isReady(): boolean
+ resetPlot(): void

#### 9. Shop
Manages weekly randomized stock and ingredient purchases.
##### Attributes:
- stock: Map<Ingredient, Integer>
- catalog: List<Ingredient>
##### Methods:
+ restockWeekly(): void (randomizes available items and quantities every 7 days)
+ buyIngredient(player: Player, item: Ingredient, qty: int): boolean
+ getAvailableStock(): Map<Ingredient, Integer>

#### 10. Counter & Customer
Manages daily customer queues and order validation.
##### Counter Attributes:
- customerQueue: Queue<Customer>
- currentCustomer: Customer
##### Counter Methods:
+ generateDailyCustomers(day: int): void
+ serveCurrentCustomer(potion: Potion, player: Player): boolean
+ nextCustomer(): Customer
##### Customer Attributes:
- name: String
- requestedEffect: String
- rewardGold: int
##### Customer Methods:
+ evaluatePotion(potion: Potion): boolean (returns true only if potion.getEffectName().equals(requestedEffect))
+ getRequestDetails(): String

## Class Interactions (How Objects Work Together, more detailed explanation of how the game works)
Daily Initialization: GameManager.startNewDay() triggers Garden.advanceDayGrowth(), Counter.generateDailyCustomers(), and—if a new week has begun—Shop.restockWeekly().
Resource Gathering: The Player either harvests mature crops from Garden (via GardenPlot.isReady() → Player.addIngredient()) or buys missing ingredients from Shop.buyIngredient(), which verifies Player.spendGold().
Brewing Loop:
The Player selects an Ingredient from their inventory (Player.useIngredient()) and specifies a grind level.
Cauldron.addIngredient() invokes polymorphism by calling ingredient.calculateMovement(grindLevel) to get a movement vector (dx, dy).
AlchemyMap.moveMarker(dx, dy) updates (markerX, markerY) and triggers Tile.onLand().
If the Tile is an EffectTile, Cauldron records the effect; if it is a BoundaryTile, Cauldron.clearCauldron() destroys the consumed ingredients and AlchemyMap.resetMarker() returns the marker to (0, 0).
Selling at the Counter: Once satisfied, the player calls Cauldron.bottlePotion() to produce a Potion object and passes it to Counter.serveCurrentCustomer(). Customer.evaluatePotion() compares the potion's effect against requestedEffect. On an exact match, Player.addGold(rewardGold) is called; regardless of outcome, Counter.nextCustomer() advances the queue.



- Describes system components, features, and expected outputs clearly and logically. NOTE: Clearly described the elements(classes), their attributes and methods and how they will interact with each other.

- Include the description as well of your target demographics as the user of your application. 

- The description is then followed by the class diagram, depicting clearly what was described.

**Sample class Diagram from https://static.visual-paradigm.com/web/images/tutorials/class-diagram-tutorial/01-class-diagram-example.png:**

![Sample Diagram](https://static.visual-paradigm.com/web/images/tutorials/class-diagram-tutorial/01-class-diagram-example.png)


Class Diagram Link
https://drive.google.com/file/d/1j2fV419mKqPRhLEzYKG7ThZ_arZWcig7/view?usp=drivesdk

### Goals:

Describe the long term outcomes of your project followed by a couple of objectives to meet the goals stated in this paragraph

- Objective 1: Be able to Create Modern games that are much more complex
- Objective 2: Teach players "chemical" combinations



[Click here for Rubrics and Sample projects](https://docs.google.com/document/d/1xKd1CqK6DcNWS2_oJPBNtY5n3I3Hqiw8FY3W295Iqfk/edit?usp=sharing)
