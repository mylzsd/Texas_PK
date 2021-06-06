# Texas_PK

### *class* utils.GameInfo
包含一场游戏的固定信息
#### Attributes:
Name|Type|Description
----|----|-----------
num_player|int|本场游戏玩家总数
init_stack|int|每位玩家的起始筹码数量
init_big_blind_amount|int|本场游戏初始大盲注数额，小盲注为该数额的一半，该数额确保能被2整除，即小盲注也为整数。随着游戏进行，盲注数额会逐渐增加。
players|\[PlayerInfo\]|每位玩家的具体信息，玩家的座位号等于其在数组中的位置
ranks|\[int\]|本场游戏的排名，以座位号对应玩家，在数组中的位置越靠前则排名越高，在每场游戏结束时显示
<br />
<br />

### *class* utils.RoundInfo
包含一局游戏的固定信息
#### Attributes:
Name|Type|Description
----|----|-----------
button|int|本局游戏发牌人座位号
small_blind|int|本局游戏小盲注座位号
big_blind|int|本局游戏大盲注座位号
big_blind_amount|int|本局游戏大盲注的数额，小盲注为该数额的一半，该数额确保能被2整除，即小盲注也为整数。大盲注的额度为本局最小的下注额度。
<br />
<br />

### *class* utils.TableState<br />
牌桌当前状态，每当有玩家进行操作或一局游戏结束时，状态会更新并广播给所有玩家（包括执行该操作的玩家）
#### Attributes:
Name|Type|Description
----|----|-----------
curr_turn|int|当前轮次，每局游戏最多会有五轮下注，分别为：<br />0 - 盲注，<br />1 - 发公共牌之前，<br />2 - 发完前三张公共牌后，<br />3 - 发完第四张公共牌后，<br />4 - 发完最后一张公共牌后，<br />5 - 筹码结算后。 
players|\[PlayerInfo\]|当前每位玩家的具体信息，玩家的座位号等于其在数组中的位置
community_cards|\[Card\]|公共牌，最少零张，最多五张，扑克牌在数组中的位置越靠前表示这张牌越早发出
curr_bet|BetInfo|本次玩家操作的信息
pots|\[PotInfo\]|当前所有筹码池信息
<br />
<br />

### *class* utils.PlayerInfo<br />
玩家信息
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
FOLDED|1|已在本局游戏中弃牌，但并未从本场游戏淘汰
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
包含玩家一次操作的信息
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

### *class* utils.PotInfo<br />
牌桌上单个筹码池的信息<br />
所有筹码池会在每局游戏开始时清空，一局游戏中所有玩家的下注都会进入筹码池。当有玩家ALLIN且后面有其他玩家的下注大于ALLIN的数额时，所有与ALLIN数额相同的部分筹码进入一个筹码池，任何超出该数额的筹码进入第二个筹码池，即第二个筹码池不包含ALLIN玩家。分配筹码时，每个筹码池只会在该池包含的玩家中进行分配。<br />
例如：<br />
玩家0下注800，玩家1 ALLIN 500，玩家2下注800。此时场上会有两个筹码池，池0包含了玩家0、1、2的各500筹码，总额为1500，池1包含玩家0和2的各300筹码，总额为600。若最后玩家0和玩家1牌面大小相等，玩家2牌面较弱，则玩家0和玩家1同时获胜。此时，玩家0和玩家1平分池0中的1500筹码，各得750；玩家1独自获得池1的600筹码。总计：玩家0 +550，玩家1 +250，玩家2 -800。
#### Attributes:
Name|Type|Description
----|----|-----------
pot_id|int|筹码池ID，等于该筹码池在数组中的位置
amount|int|该筹码池当前总额
players|\[int\]|该筹码池包含玩家的座次
winners|\[int\]|该筹码池获胜玩家的座次，当每局结束时显示，平手时会包含多名玩家，不分先后
<br />
<br />

### *class* utils.BasePlayer<br />
玩家需要实现一个名为Player的类，并继承此类。<br />
下列函数可在Player类中覆盖，但是接口参数和返回类型必须保持一致。
#### Functions:

****

```Python
__init__(self, name: str)
```
##### Input：<br />
Name|Type|Description
----|----|-----------
name|str|名字，用于标识玩家，且不会关联现实中的身份。Player对象的生命周期为一或多场游戏，在这期间，名字保持不变，并对所有人可见，可用于记录玩家的行为模式。

****

```Python
game_start(self, game_info: GameInfo) -> None
```
##### Input：<br />
Name|Type|Description
----|----|-----------
game_info|GameInfo|本场游戏开始时的信息，可参考GameInfo类。此时players中所有元素均为初始状态，ranks无效，后续实时状态在TableSate中更新。

****

```Python
game_end(self, game_info: GameInfo) -> None
```
##### Input：<br />
Name|Type|Description
----|----|-----------
game_info|GameInfo|本场游戏结束时的信息，可参考GameInfo类。此时players为最后状态；ranks表示本场游戏最终排名。

****

```Python
round_start(self, round_info: RoundInfo) -> None
```
##### Input：<br />
Name|Type|Description
----|----|-----------
round_info|RoundInfo|本局游戏基本信息，参考RoundInfo类。

****

```Python
round_end(self, table_state: TableState) -> None
```
筹码结算后通过此函数将桌面状态同步给所有玩家，包括已淘汰的玩家。
##### Input：<br />
Name|Type|Description
----|----|-----------
table_state|TableState|本局游戏结束时桌面的信息。其中各玩家的筹码为结算后的数额，curr_bet无效，pots中的的winners此时有效。

****

```Python
update_state(self, table_state: TableState) -> None
```
任何玩家操作后会通过此函数将桌面状态同步给所有玩家，包括执行该操作的玩家自身。
##### Input：<br />
Name|Type|Description
----|----|-----------
table_state|TableState|参考TableState类。此时pots中的winners无效。

****

```Python
blind_bet(self, is_big_blind: bool, amount: int) -> None
```
当轮到玩家下盲注时，通过此函数通知玩家。此函数无需返回数值，游戏会自动计算玩家当前筹码。
##### Input：<br />
Name|Type|Description
----|----|-----------
is_big_blind|bool|True - 此次下注为大盲注<br />False - 此次下注为小盲注
amount|int|盲注的数额

****

```Python
set_cards(self, card_a: Card, card_b: Card) -> None
```
在盲注之后每位玩家发两张私有牌，除最终比大小时须公开，其他时间私有牌仅玩家自己可见。
##### Input：<br />
Name|Type|Description
----|----|-----------
card_a|Card|第一张扑克牌
card_b|Card|第二张扑克牌

****

```Python
get_action(self, table_state: TableState) -> BetInfo
```
当轮到玩家操作（下注、加注、弃牌）时，通过此函数获得玩家的操作信息。
##### Input：<br />
Name|Type|Description
----|----|-----------
tabel_state|TableState|执行操作时的桌面状态，此状态于最后一次广播同步时的相同。

##### Output：<br />
Type|Description
----|-----------
BetInfo|TODO

<br />
<br />

