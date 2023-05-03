As of Traffic Control 1.0.0, four new OpenComputers cards are available when you have OpenComputers installed. The purpose of these cards is to control traffic lights remotely and without the need of a traffic light control box. These cards are the `Traffic Control Cards` with Tiers 1-3 as well as a Creative card.

# Tier Limits
Each tier has a limit on how many traffic light frames you can load onto a card. By default, these limits are:

| Tier | Max Slots |
|------|-----------|
| 1    | 16        |
| 2    | 144       |
| 3    | 384       |
| Creative | 2,147,483,647 |

These slot limits are configurable in Traffic Control's config file.

# Adding/Removing Traffic Lights
To add traffic lights to your card, simply hold the card in your hand and right click the traffic light you want to add. You will receive a chat message when you have successfully added a traffic light. To remove a traffic light, right click the traffic again with the card in hand and you will receive a chat message that you have removed the traffic light.

# OpenComputers Methods
Once you have added all of your traffic lights to your card, place the card in the appropriate slot within the computer. You will be able to access it from the `components` api by the name `traffic_light_card` or by it's UUID assigned by OpenComputers. These are the following callbacks:

| Method Name      | Return Type | Description | Uses Power |
|------------------|-------------|-------------|------------|
| `listBlockIDs()` | `array` of block position IDs | Gets a list of paired block positions as `long` IDs | false |
| `listBlockPos()` | `array` of block position `array`s (x,y,z accordingly) | Gets a list of paired block positions as `array` (x,y,z) values | false |
| `states`         | `table` | Available states and their internal value. Called like a property, not a method. | false |
| `setState(x:int, y:int, z:int, state:string, active:boolean, flash:boolean)` OR <br /> `setState(id:long, state:string, active:boolean, flash:boolean)` | `success:boolean[, errorMessage:string]` | Sets the given state on the given traffic light. | true |
| `clearStates(x:int, y:int, z:int)` OR <br /> `clearStates(id:long)` | `success:boolean[, errorMessage:string]` | Clears all active states of the traffic light | true |
| `getStates(x:int, y:int, z:int)` OR <br /> `getStates(id:long)` | `success:boolean[, errorMessage:string]` OR <br /> ``success:boolean[, states:table]` | Gets a table of all providable states (as described in the `states` property). The key of the table is the state, and the value is another table where the keys are `active` and `flash`, referring to states set in the `setState` method | true |

## Power Usage
Note that some methods use power. The amount of power used is configurable in Traffic Control's config file. This value multiplied by the distance the traffic light is away from the computer will be the amount of RF used for the call. The default value is `0.01`, or 1RF/100 blocks.