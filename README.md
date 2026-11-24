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
Represents Growing plants over time so the player has renewable ingredients for brewing. You can water these plants to maintain their health and state, tracking their growth progress and stage. harvest them once mature to obtain ingredients for your inventory.

#### The Alchemy Map:
A 2D board filled with hidden potion effects. It tracks where your potion marker is located and checks if your potion has obtained an effect. You pick and use ingredients to move your potion marker across the board toward target effects (effect tiles), but if you land on a boundary tile, your potion will be fully destabilized and you will have to restart.

#### The Brewing Process:
Includes harvested herbs and water. Each ingredient has a specific movement direction and can be ground down to travel further across the brewing map. Water is a key part to this, as it will bring back your potion marker back to where it came from (depending on the water's numeric value).

#### The Counter: 
The Counter is where you sell the requested potions to each customer. If the potion doesn't meet the standards of the customer's requests, they do not pay you any gold and will leave. If you do fully meet their standards, they pay you in full.

#### The Shop: 
The Shop is where you buy ingredients that are out of stock, and it only accepts gold. Each ingredient in the shop has a specific amount of stock until it is sold out; the shop will be randomized every game week. 

### How the Game works:
At the start of each day, customers will line up at the counter and each customer has a request potion for you to brew. To brew a potion you shall access the brewing station wherein the ingredients in your inventory, the alchemy map, and the cauldron are present. Your goal is to reach the effect tile asked by the customer. The default x and y value of the potion marker on the map is (0, 0). If you add an ingredient, its stored x and y values are added to the current potion marker's coordinates. Adding further, you can grind these ingredients to further fluctuate the coordinate values. If you reach a boundary tile, the potion explodes and your potion marker will be set back to its default x and y coordinates. Your ingredients that have been used in the potion will not be returned. If you land on an effect tile, you can choose to sell this potion with the acquired tile to the customer, or continue moving the potion marker. Once sold, the customer evaluates if the potion they were asking is the exact same potion that they were given. Even if the customer is dissatisfied or not, they will leave and the next customer shall be asked for their request.

### Class Specifications:

#### 1. Calendar
Tracks in-game dates, time progression, and daily/weekly states.
##### Attributes:
- months: list
- day_states: list
- month_id: int
- day: int
- day_count: int
- time_index: int
##### Methods:
+ current_month(self)
+ real_date(self)
+ current_day_state(self)
+ advance_time(self)
+ advance_date(self)

#### 2. GameManager (Controller)
Coordinates the game loop, day/week progression, and communication between subsystems.
##### Attributes:
- current_day: int
- current_week: int
- calendar: Calendar
- player: Player
- alchemy_map: AlchemyMap
- cauldron: Cauldron
- shop: Shop
- counter: Counter
##### Methods:
+ startGame(self)
+ startNewDay(self): aadvances time/date, updates plants, loads daily customers, and randomizes shop stock every game week
+ end_day(self)
+ brew_ingredient(self, item, grind_level)
+ sell_to_customer(self)

#### 3. Player
Represents the alchemist's resources and inventory.
##### Attributes:
- gold: int
- inventory: dict
- potions: List<Potion>
##### Methods:
+ add_gold(self, amount: int)
+ spend_gold(self, amount: int): boolean
+ add_ingredient(self, item, count: int): void
+ consume_ingredient(self, item): boolean


#### 4. Ingredient (Abstract Base Class)
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

#### 5. Plant
Represents a growable plant in the garden that can also be ground/harvested for alchemy use.
##### Attributes:
- id: int
- name: string
- watering_time: int
- water_state: string
- health: int
- growtime: int
- growth: int
- time_no_water: int
- logs: list
- g1: int
- g2: int
##### Methods:
+ grind1(self)
+ grind2(self)
+ water(self)
+ harvest(self)
+ update(self)
+ log(self)
+ stage(self)

#### 6. Water (Subclass of Ingredient)
Pulls the potion marker back toward (0, 0) proportionally to its numeric pull.
##### Attributes:
- pull_strength: int
##### Methods:
+ get_movement(self, grind_level): Vector2D (returns a vector moving toward (0, 0) capped by dilutionValue)

#### 7. AlchemyMap & Tile Hierarchy
Manages the 2D board, hidden effects, and hazard boundaries.
##### AlchemyMap Attributes:
- rows: int
- cols: int
- tiles: list
- marker_x: int (default 0)
- marker_y: int (default 0)
##### AlchemyMap Methods:
+ step_marker(self, dx, dy): Tile
+ reset_marker(self): void (sets markerX = 0, markerY = 0)
+ get_tile(self, x, y): Tile

> Tile (Base Class):
- x: int
- y: int
- revealed: boolean
+ trigger(self, cauldron, map)

> EffectTile (Subclass of Tile):
- effect: String
+ trigger(cauldron, map): String (reveals tile and attaches trigger to the active potion)

> BoundaryTile (Subclass of Tile):
+ trigger(cauldron, map): String (destabilizes potion, clears cauldron, calls map.reset_marker())

#### 8. Cauldron & Potion
Handles active brewing state and produces finished potions.
##### Cauldron Attributes:
- ingredients_used: List<Ingredient>
- current_effect: String
- ruined: boolean
##### Cauldron Methods:
+ add_to_brew(self, item, grind_level, map)
+ finish_potion(self): Potion
+ dump_brew(self): void (resets ingredients without returning them to player inventory)
##### Potion Attributes:
- effect: String
- value: int
##### Potion Methods:
+ get_effect(self): String
+ get_value(self): int

#### 9. Shop
Manages weekly randomized stock and ingredient purchases.
##### Attributes:
- stock: dict
- item_pool: List<Ingredient>
##### Methods:
+ randomize_weekly_stock(self): void (randomizes available items every game week)
+ purchase(self, player, item, qty): boolean

#### 10. Counter & Customer
Manages daily customer queues and order validation.
##### Counter Attributes:
- queue: list<Customer>
- active_customer: Customer
##### Counter Methods:
+ load_customers_for_day(self, day)
+ hand_over_potion(self, potion, player): boolean
+ advance_queue(self)
##### Customer Attributes:
- name: String
- desired_effect: String
- payout: int
##### Customer Methods:
+ check_order(self, potion): boolean (returns true only if potion.get_effect().equals(desired_effect))


## Class Interactions (How Objects Work Together, more detailed explanation of how the game works)
Daily & Weekly Progression: GameManager.start_new_day() coordinates with Calender (advance_date / advance_time), updates Plant states (update), calls Counter.load_customers_for_day(day), and calls Shop.randomize_weekly_stock() every game week.
Resource Gathering: The player can tend and harvest Plant objects (water, harvest) into Player.inventory (add_ingredient), or buy items from Shop.purchase(player, item, qty) using Player.spend_gold(amount).
Brewing Loop:
GameManager.brew_ingredient(item, grind_level) consumes the ingredient from the player (Player.consume_ingredient(item)) and adds it to the cauldron (Cauldron.add_to_brew(item, grind_level, map)).
Ingredient.get_movement(grind_level) computes the movement shift (Plant or Water), and AlchemyMap.step_marker(dx, dy) updates marker_x and marker_y.
Landing on a tile calls Tile.trigger(cauldron, map): an EffectTile reveals itself and sets current_effect on Cauldron, while a BoundaryTile calls Cauldron.dump_brew() (without returning used ingredients) and AlchemyMap.reset_marker() back to (0, 0).
Selling in the Counter: When ready, Cauldron.finish_potion() produces a Potion, and GameManager.sell_to_customer() passes it to Counter.hand_over_potion(potion, player). Customer.check_order(potion) compares potion.get_effect() against desired_effect; if matched, Player.add_gold(payout) is called. Regardless of whether the order succeeded, Counter.advance_queue() moves to the next customer.




### Goals:

Describe the long term outcomes of your project followed by a couple of objectives to meet the goals stated in this paragraph

- Objective 1: Be able to Create Modern games that are much more complex
- Objective 2: Teach players "chemical" combinations



[Click here for Rubrics and Sample projects](https://docs.google.com/document/d/1xKd1CqK6DcNWS2_oJPBNtY5n3I3Hqiw8FY3W295Iqfk/edit?usp=sharing)
