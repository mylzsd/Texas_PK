# Texas_PK

### utils.BasePlayer<br />
#### Attributes:
Name|Type|Description
----|----|-----------
name|str|玩家ID，不体现真实身份，在同一桌的多次对局中保持一致
stack|int|当前筹码
cards|[Card]|当前手牌

#### Functions:
```Python
__init__(self, name: str) -> BasePlayer
```
<br />
<br />

### utils.Card<br />
#### Attributes:
Name|Type|Description
----|----|-----------
suit|str|花色，共四种: 'spades'-黑桃, 'hearts'-红桃, 'clubs'-梅花, 'diamonds'-方块
rank|int|数值，1-13

#### Function:
```Python
__init__(self, suit: str, rank: int) -> Card
```
<br />
<br />

### utils.PlayerInfo<br />
#### Attributes:
Name|Type|Description
----|----|-----------

### utils.State<br />
#### Attributes:
Name|Type|Description
----|----|-----------
num_player|int|本局玩家总数

