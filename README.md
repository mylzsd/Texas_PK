# Texas_PK

### *class* utils.GameInfo
#### Attributes:
Name|Type|Description
----|----|-----------
num_player|int|本场游戏玩家总数
init_stack|int|每位玩家的起始筹码数量
players|\[PlayerInfo\]|每位玩家的具体信息，玩家的座次等于其在数组中的位置
<br />
<br />

### *class* utils.RoundInfo
#### Attributes:
Name|Type|Description
----|----|-----------
button|int|本局游戏发牌人座次
small_blind|int|本局游戏小盲注座次
big_blind|int|本局游戏大盲注座次
<br />
<br />

### *class* utils.TableState<br />
牌桌当前状态，每当有玩家进行操作或一局游戏结束时，状态会更新并广播给所有玩家（包括执行该操作的玩家）
#### Attributes:
Name|Type|Description
----|----|-----------
curr_turn|int|当前轮次，每局游戏最多会有五轮下注，分别为：<br />0 - 盲注，<br />1 - 发公共牌之前，<br />2 - 发完前三张公共牌后，<br />3 - 发完第四张公共牌后，<br />4 - 发完最后一张公共牌后 
players|\[PlayerInfo\]|当前每位玩家的具体信息，玩家的座次等于其在数组中的位置
community_cards|\[Card\]|公共牌，最少零张，最多五张，扑克牌在数组中的位置越靠前表示这张牌越早发出
curr_bet|BetInfo|本次玩家操作的信息
pots|\[PotInfo\]|当前所有筹码池信息
<br />
<br />

### *class* utils.PlayerInfo<br />
#### Attributes:
Name|Type|Description
----|----|-----------
name|str|玩家ID，不体现真实身份，在同一桌的多场游戏中保持一致
seat|int|玩家在本场游戏中的座位号
state|PlayerState|玩家当前状态
stack|int|玩家当前持有的筹码
cards|\[Card\]|玩家手牌，在每局下注时隐藏，直到最后开牌比大小时显示
<br />
<br />

### *enum* utils.PlayerState<br />
#### Values：
Name|Value|Description
----|-----|-----------
PLAYING|0|正在参与本局游戏
FOLD|1|已在本局游戏中弃牌，但并未从本场游戏淘汰
OUT|2|已从本场游戏中淘汰（筹码归零或犯规）
<br />
<br />

### *class* utils.Card<br />
#### Attributes:
Name|Type|Description
----|----|-----------
suit|Suit|花色，共四种: 'spades'-黑桃, 'hearts'-红桃, 'clubs'-梅花, 'diamonds'-方块
rank|int|数值，1-13。1->A, 11->J, 12->Q, 13->K
<br />
<br />


### *enum* utils.Suit<br />
#### Values：
Name|Value|Description
----|-----|-----------
SPADES|0|黑桃
HEARTS|1|红桃
CLUBS|2|梅花
DIAMONDS|3|方块
<br />
<br />

### *class* utils.BetInfo<br />
包含一次玩家操作相关的信息
#### Attributes:
Name|Type|Description
----|----|-----------
player_seat|int|玩家的座次
action|Action|操作类型
amount|int|下注的额度，当action为BET或ALLIN时此参数才有意义，其他情况下为don't care
<br />
<br />

### *enum* utils.ACTION
#### Values：
Name|Value|Description
----|-----|-----------
BET|0|普通下注，需要加上额度
ALLIN|1|下注额为当前持有的全部筹码
FOLD|2|弃牌，棋牌后不得参与本局后续的下注，已投入的筹码不能取回
BLIND|3|盲注，游戏规定的强制下注
TIMEOUT|4|玩家处理超时，直接淘汰
ILLEGAL|5|玩家下注额度不符合规则，直接淘汰
<br />
<br />

### utils.PotInfo<br />
牌桌上单个筹码池的信息，所有筹码池会在每局游戏开始时清空，一局游戏中所有玩家的下注都会进入筹码池。
当有玩家ALLIN且后面有其他玩家的下注大于ALLIN的数额时，所有与ALLIN数额相同的部分筹码进入一个筹码池，任何超出该数额的筹码进入第二个筹码池，即第二个筹码池不包含ALLIN玩家。
分配筹码时，每个筹码池只会在该池包含的玩家中进行分配。
例如：玩家0下注800，玩家1 ALLIN 500，玩家2下注800。此时场上会有两个筹码池，池0包含了玩家0、1、2各500的筹码，总额为1500；池1包含玩家0的300和玩家2的300筹码，总额为600。若最后玩家0和玩家1牌面大小相等，玩家2牌面较弱，则玩家0和玩家1同时获胜。此时，玩家0和玩家1平分池0中的1500筹码，各得750；玩家1独自获得池1的600筹码。总计：玩家0 +550，玩家1 +250， 玩家2 -800。
#### Attributes:
Name|Type|Description
----|----|-----------
id|int|筹码池ID，等于该筹码池在数组中的位置
amount|int|该筹码池当前总额
players|\[int\]|该筹码包含玩家的座次编号，当有玩家allin后，超出allin部分的筹码池将不包含该玩家
winners|\[int\]|该筹码池获胜玩家座次，当每轮结束时显示，平手时会包含多名玩家，不分先后
<br />
<br />

### utils.BasePlayer<br />
#### Attributes:
Name|Type|Description
----|----|-----------
name|str|玩家ID，不体现真实身份，在同一桌的多次对局中保持一致
seat|int|玩家当前座次
stack|int|当前筹码
cards|[Card]|当前手牌

# TODO
#### Functions:
```Python
__init__(self, name: str)
```
```Python
game_start(self, game_info: GameInfo) -> None
```
```Python
game_end(self, game_info: GameInfo) -> None
```
```Python
round_start(self, state: State) -> None
```
```Python
round_end(self, state: State) -> None
```
```Python
set_stack(self, stack: int) -> None
```
<br />
<br />

