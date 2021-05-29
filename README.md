# Texas_PK

### utils.SUITS
\['spades', 'hearts', 'clubs', 'diamonds'\]
<br />
牌面花色，共四种：'spades'-黑桃, 'hearts'-红桃, 'clubs'-梅花, 'diamonds'-方块
<br />
<br />
### utils.RANKS
list(range(1, 14))
<br />
牌面数值，1-13。1->A, 11->J, 12->Q, 13->K
<br />
<br />
### utils.ACTIONS
\['bet', 'allin', 'fold', 'blind', 'timeout', 'illegal'\]
<br />
几种不同的下注行为，其中玩家的callback只用'bet'和'fold'。其余的会在BetInfo中用来体现操作类型。
- bet：普通下注，需要加上额度
- allin：下注额为当前持有的全部筹码
- fold：弃牌，棋牌后不得参与本轮之后的下注，已投入的筹码不能取回
- blind：盲注，游戏流程中轮巡强制下注
- timeout：玩家处理超时，直接淘汰
- illegal：玩家下注额度不符合规则，直接淘汰
<br />
<br />

### utils.Card<br />
#### Attributes:
Name|Type|Description
----|----|-----------
suit|str|花色，共四种: 'spades'-黑桃, 'hearts'-红桃, 'clubs'-梅花, 'diamonds'-方块
rank|int|数值，1-13
<br />
<br />

### utils.BasePlayer<br />
#### Attributes:
Name|Type|Description
----|----|-----------
name|str|玩家ID，不体现真实身份，在同一桌的多次对局中保持一致
stack|int|当前筹码
cards|[Card]|当前手牌

# TODO
#### Functions:
```Python
__init__(self, name: str) -> BasePlayer
```
<br />
<br />

### utils.PlayerInfo<br />
#### Attributes:
Name|Type|Description
----|----|-----------
name|str|玩家ID，不体现真实身份，在同一桌的多次对局中保持一致
seat|int|当前座位号
stack|int|当前筹码
cards|\[Card\]|当前手牌，在每轮的下注中隐藏，在最后开牌比大小时显示
<br />
<br />

### utils.PotInfo<br />
#### Attributes:
Name|Type|Description
----|----|-----------
id|int|筹码池ID
amount|int|该筹码池当前总额
players|\[int\]|该筹码包含玩家的座次编号，当有玩家allin后，超出allin部分的筹码池将不包含该玩家
<br />
<br />

### utils.BetInfo<br />
#### Attributes:
Name|Type|Description
----|----|-----------
player_seat|int|下注玩家的座次
action|str|utils.ACTIONS中的任意一种
amount|int|本次下注的金额，当action为'bet'或'allin'时此参数有意义

#### Function:
```Python
__init__(self, suit: str, rank: int) -> Card
```
<br />
<br />

### utils.State<br />
#### Attributes:
Name|Type|Description
----|----|-----------
num_player|int|本局玩家总数

players|\[PlayerInfo\]|根据座次依次显示玩家信息




