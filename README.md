# liuheChess

![](https://github.com/chessgame/liuheChess/blob/master/bg.png)



### 六洲棋

六洲棋，又称：泥棋、插方、来马、五福、六合棋，中国民间传统棋类体育形式。源于民间，简便、通俗、易学，在民间广为流行，深受社会底层大众的喜爱。龙其在淮河流域的安徽省、河南省、江苏省、以及湖北省、山东省非常普及，并流传到中国各地，包括港、澳、台地区。起源于劳动人民生活，根植于民间大众之中，它简捷、明快，趣味性、竞技性强，是一项长期流行于民间，富有传统文化色彩的竞技项目。对于启迪智慧，休闲娱乐，增进交流非常有益。列安徽省第二批省级非物质文化遗产。
6*6纵横线组成，共三十六个棋点。每方十八枚棋子，以两色区分敌我。

#### 规则

对弈过程分三阶段。（凤阳下法）放子：对弈双方依次将己子放入空棋点，将手上的棋子放完才开始走子。逼子：若无棋子被吃，使得棋子放满棋盘。则两人各选对方一枚敌子移出游戏。走子：由后手方开始轮流移动己棋，沿线直横线一格。吃子：无论是下子或走子阶段，只要己方棋子排成以下排列称为成城，就要吃掉一定数量的敌子，但不可吃掉已成城子的敌棋。在放子阶段，被吃的子先作记号，等走子阶段开始才一齐提取。
成六：六枚棋子以纵、横和斜3个方向连成直线(除了四条边的直线)。吃掉敌方三子。

![six](http://img.blog.csdn.net/20170803145822176?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGlhbmppZm91/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![six_two](http://img.blog.csdn.net/20170803145840358?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGlhbmppZm91/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

斜五：连子的2头都靠棋盘边缘，吃掉敌方两子。

![five](http://img.blog.csdn.net/20170803145906750?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGlhbmppZm91/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

斜四：连子的2头都靠棋盘边缘，吃掉敌方一子。

![four](http://img.blog.csdn.net/20170803145922915?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGlhbmppZm91/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

斜三：连子的2头都靠棋盘边缘，吃掉敌方一子。

![three](http://img.blog.csdn.net/20170803145939228?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGlhbmppZm91/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

成方：四枚棋子组成一个紧邻相连的小正方形，吃掉敌方一子。


![check](http://img.blog.csdn.net/20170803150001736?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGlhbmppZm91/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

使对方只剩下三枚以下则获胜。因为是民间文化，各地稍有差异。

### 五子棋

五子棋五子棋是比较流行的棋类游戏了，玩法简单，基本上人人会玩，在此就不介绍游戏规则了，在此介绍下app中的AI的实现思路，主要实现的AI算法，包括极大值极小值算法，深度搜索算法，估值函数，Alpha Beta 剪枝算法等等。
```swift
//横向五子连珠（除去四边线的五子连珠）
static func isFiveChess(_ point:SWSPoint,_ chessArray: [[FlagType]]) -> Bool {
let type = chessArray[point.x][point.y]
let pointLeft = SWSPoint()
let pointRight = SWSPoint()
let pointTop = SWSPoint()
let pointBottom = SWSPoint()
let pointLeft45 = SWSPoint()
let pointRight45 = SWSPoint()
let pointTop135  = SWSPoint()
let pointBottom135 = SWSPoint()
//东西方向
var i = 0
while point.x - i >= 0 && chessArray[point.x - i][point.y] == type {
pointLeft.x = point.x - i
i += 1
}
i = 0
while point.x + i <= 14 && chessArray[point.x + i][point.y] == type {
pointRight.x = point.x + i
i += 1
}

if pointRight.x - pointLeft.x == 4 && (pointLeft.y != 15 || pointLeft.y != 0){
return true
}
//南北方向
i = 0
while point.y - i >= 0 && chessArray[point.x][point.y-i] == type {
pointTop.y = point.y - i
i += 1
}
i = 0
while point.y + i <= 14 && chessArray[point.x][point.y+i] == type {
pointBottom.y = point.y + i
i += 1
}
if pointBottom.y - pointTop.y == 4 && (pointTop.x != 15 || pointTop.x != 0) {
return true
}

// 东北方向
i = 0
while point.x - i >= 0 && point.y + i <= 14 && chessArray[point.x - i][point.y + i] == type {
pointLeft45.x = point.x - i
pointLeft45.y = point.y + i
i += 1
}
i = 0
while point.x + i <= 14 && point.y - i >= 0 && chessArray[point.x + i][point.y - i] == type {
pointRight45.x = point.x + i
pointRight45.y = point.y - i
i += 1
}

if pointLeft45.y - pointRight45.y == 4{
return true
}

//西北方向
i = 0
while point.x - i >= 0 && point.y - i >= 0 && chessArray[point.x - i][point.y - i] == type {
pointTop135.x = point.x - i
pointTop135.y = point.y - i
i += 1
}
i = 0
while point.x + i <= 14 && point.y + i <= 14 && chessArray[point.x + i][point.y + i] == type {
pointBottom135.x = point.x + i
pointBottom135.y = point.y + i
i += 1
}
if pointBottom135.y - pointTop135.y == 4{
return true
}

return false
}

```
#### 五子棋的AI算法实现

2017年互联网最火的技术毫无疑问就是AI了，在此尝试写了个算法来和人脑来pk。五子棋属于零和游戏：一方胜利代表另一方失败，而零和游戏的代表算法就是极大值极小值搜索算法。

#### 极大值极小值搜索算法

A、B二人对弈，A先走，A始终选择使局面对自己最有利的位置，然后B根据A的选择，在剩下的位置中选择对A最不利的位置，以此类推下去直到到达我们定义的最大搜索深度。所以每一层轮流从子节点选择最大值－最小值－最大值－最小值...

![这里写图片描述](http://img.blog.csdn.net/20170803144141913?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGlhbmppZm91/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

我们如何知道哪个位置最有利和最不利呢？在此我们引入一套评估函数，来对棋盘上每个位置进行分数评估

```swift
//活一、活二、活三、活四、连五、眠一，眠二、眠三、眠四
enum FiveChessType:Int {
case liveOne = 0
case liveTwo
case liveThree
case liveFour
case liveFive
case sleepOne
case sleepTwo
case sleepThree
case sleepFour
case unknown
var score:Int  {
switch self {
case .unknown:
return un_known
case .sleepOne:
return sleep_One
case .liveOne,.sleepTwo:
return live_One
case .liveTwo,.sleepThree:
return live_Two
case .liveThree:
return live_Three
case .sleepFour:
return sleep_Four
case .liveFour:
return live_Four
case .liveFive:
return live_Five

}
}

}
let live_Five = 1000000
let live_Four = 100000
let sleep_Four = 10000
let live_Three = 1000
let live_Two = 100
let sleep_Three = 100
let live_One = 10
let sleep_Two = 10
let sleep_One = 1
let un_known = 0
```
在使用极大值极小值进行深度搜索时，遍历节点是指数增长的，如果不进行算法优化，将会导致电脑计算时间过长，影响下棋体验，所以这里引入 Alpha Beta 剪枝原理。

#### Alpha Beta 剪枝原理

AlphaBeta剪枝算法是一个搜索算法旨在减少在其搜索树中，被极大极小算法评估的节点数。
Alpha-Beta只能用递归来实现。这个思想是在搜索中传递两个值，第一个值是Alpha，即搜索到的最好值，任何比它更小的值就没用了，因为策略就是知道Alpha的值，任何小于或等于Alpha的值都不会有所提高。
第二个值是Beta，即对于对手来说最坏的值。这是对手所能承受的最坏的结果，因为我们知道在对手看来，他总是会找到一个对策不比Beta更坏的。如果搜索过程中返回Beta或比Beta更好的值，那就够好的了，走棋的一方就没有机会使用这种策略了。
在搜索着法时，每个搜索过的着法都返回跟Alpha和Beta有关的值，它们之间的关系非常重要，或许意味着搜索可以停止并返回。
如果某个着法的结果小于或等于Alpha，那么它就是很差的着法，因此可以抛弃。因为我前面说过，在这个策略中，局面对走棋的一方来说是以Alpha为评价的。
如果某个着法的结果大于或等于Beta，那么整个节点就作废了，因为对手不希望走到这个局面，而它有别的着法可以避免到达这个局面。因此如果我们找到的评价大于或等于Beta，就证明了这个结点是不会发生的，因此剩下的合理着法没有必要再搜索。
如果某个着法的结果大于Alpha但小于Beta，那么这个着法就是走棋一方可以考虑走的，除非以后有所变化。因此Alpha会不断增加以反映新的情况。有时候可能一个合理着法也不超过Alpha，这在实战中是经常发生的，此时这种局面是不予考虑的，因此为了避免这样的局面，我们必须在博弈树的上一个层局面选择另外一个着法。[链接](http://www.xqbase.com/computer/search_alphabeta.htm)

c代码实现原理

```c
int AlphaBeta(int depth, int alpha, int beta) 
{
if (depth == 0) 
{
return Evaluate();
}
GenerateLegalMoves();
while (MovesLeft()) 
{
MakeNextMove();
val = -AlphaBeta(depth - 1, -beta, -alpha);
UnmakeMove();
if (val >= beta) 
{
return beta;
}
if (val > alpha) 
{
alpha = val;
}
}
return alpha;
} 

```


```swift
static func getAIPoint(chessArray:inout[[FlagType]],role:FlagType,AIScore:inout [[Int]],humanScore:inout [[Int]],deep:Int) ->(Int,Int,Int)? {

let maxScore = 10*live_Five
let minScore = -1*maxScore
let checkmateDeep = self.checkmateDeep
var total=0, //总节点数
steps=0,  //总步数
count = 0,  //每次思考的节点数
ABcut = 0 //AB剪枝次数


func humMax(deep:Int)->(Int,Int,Int)? {
let points = self.getFiveChessType(chessArray: chessArray, AIScore: &AIScore, humanScore: &humanScore)
var bestPoint:[(Int,Int)] = []
var best = minScore
count = 0
ABcut = 0

for i in 0..<points.count {
let p = points[i]
chessArray[p.x][p.y] = role
self.updateOneEffectScore(chessArray: chessArray, point: (p.x,p.y), AIScore: &AIScore, humanScore: &humanScore)
var score = -aiMaxS(deep: deep-1, alpha: -maxScore, beta: -best, role: self.reverseRole(role: role))
if p.x < 3 || p.x > 11 || p.y < 3 || p.y > 11 {
score = score/2
}
if TJFTool.equal(a: Float(score), b: Float(best)){
bestPoint.append((p.x,p.y))
}
if TJFTool.greatThan(a: Float(score), b: Float(best)){
best = score
bestPoint.removeAll()
bestPoint.append((p.x,p.y))
}
chessArray[p.x][p.y] = .freeChess
self.updateOneEffectScore(chessArray: chessArray, point: (p.x,p.y), AIScore: &AIScore, humanScore: &humanScore)

}
steps += 1
total += count
if bestPoint.count > 0 {
let num = arc4random()%UInt32(bestPoint.count)
return (bestPoint[Int(num)].0,bestPoint[Int(num)].1,best)
}
return nil

}

func aiMaxS(deep:Int,alpha:Int,beta:Int,role:FlagType) -> Int{
var score = 0
var aiMax = 0
var humMax = 0
var best = minScore
for i in 0..<15{
for j in 0..<15{
if chessArray[i][j] == .freeChess{
aiMax = max(AIScore[i][j], aiMax)
humMax = max(humanScore[i][j], humMax)
}
}
}
score = (role == .blackChess ? 1 : -1) * (aiMax-humMax)
count += 1
if deep <= 0 || TJFTool.greatOrEqualThan(a: Float(score), b: Float(live_Five)){
return score
}
let points =  self.getFiveChessType(chessArray: chessArray, AIScore: &AIScore, humanScore: &humanScore)
for i in 0..<points.count{
let p = points[i]
chessArray[p.x][p.y] = role
self.updateOneEffectScore(chessArray: chessArray, point: (p.x,p.y), AIScore: &AIScore, humanScore: &humanScore)
let some = -aiMaxS(deep: deep-1, alpha: -beta, beta: -1 * ( best > alpha ? best : alpha), role: self.reverseRole(role: role)) * deepDecrease
chessArray[p.x][p.y] = .freeChess
self.updateOneEffectScore(chessArray: chessArray, point: (p.x,p.y), AIScore: &AIScore, humanScore: &humanScore)
if TJFTool.greatThan(a: Float(some), b: Float(best)) {
best = some
}
//在这里进行ab 剪枝
if TJFTool.greatOrEqualThan(a: Float(some), b: Float(beta)){
ABcut += 1
return some
}
}

if (deep == 2 || deep == 3 || deep == 4) && TJFTool.littleThan(a: Float(best), b: Float(sleep_Four)) && TJFTool.greatThan(a: Float(best), b: -(Float)(sleep_Four)){

if let result = self.checkmateDeeping(chessArray: &chessArray, role: role, AIScore: &AIScore, humanScore: &humanScore, deep: checkmateDeep) {
return Int(Double(result[0].2) * pow(0.8, Double(result.count)) * (role == .blackChess ? 1:-1))
}
}
return best
}

var i = 2
var result:(Int,Int,Int)?
while i <= deep {
if let test = humMax(deep: i) {
result = test
if TJFTool.greatOrEqualThan(a: Float(test.2), b: Float(live_Four)) {
return test
}
}
i += 2
}
if result == nil {
var maxAiScore = 0
for i in 0..<15{
for j in 0..<15 {
if chessArray[i][j] == .freeChess && maxAiScore < AIScore[i][j] {
maxAiScore = AIScore[i][j]
result = (i,j,maxAiScore)
}
}
}
}

return result
}
```
经过Alpha Beta剪枝后，优化效果应该达到 1/2 次方，也就是说原来需要遍历X^Y个节点，现在只需要遍历X^(Y/2)个节点，相比之前已经有了极大的提升。
不过即使经过了Alpha Beta 剪枝，思考层数也只能达到四层，也就是一个不怎么会玩五子棋的普通玩家的水平。而且每增加一层，所需要的时间或者说计算的节点数量是指数级增加的。所以目前的代码想计算到第六层是很困难的。
我们的时间复杂度是一个指数函数 X^Y，其中底数X是每一层节点的子节点数，Y 是思考的层数。我们的剪枝算法能剪掉很多不用的分支，相当于减少了 Y，那么下一步我们需要减少 X，如果能把 X 减少一半，那么四层平均思考的时间能降低到 0.5^4 = 0.06 倍，也就是能从10秒降低到1秒以内。
如何减少X呢？我们知道五子棋中，成五、活四、双三、双眠四、眠四活三是必杀棋，于是我们遇到后就不用再往下搜索了。代码如下：

```swift
static func getFiveChessType(chessArray:[[FlagType]],AIScore:inout [[Int]],humanScore:inout [[Int]]) ->[(x:Int,y:Int)]{
var twos:[(Int,Int)] = []
var threes:[(Int,Int)] = []
var doubleThrees:[(Int,Int)] = []
var sleepFours:[(Int,Int)] = []
var fours:[(Int,Int)] = []
var fives:[(Int,Int)] = []
var oters:[(Int,Int)] = []
for i in 0..<15{
for j in 0..<15{
if chessArray[i][j] == .freeChess && self.effectivePoint(chessArray: chessArray, point: (x: i, y: j)) {
let aiScore = AIScore[i][j]
let humScore = humanScore[i][j]
if aiScore>=live_Five {
return[(i,j)]
}else if humScore >= live_Five {
fives.append((i,j))
}else if aiScore >= live_Four {
fours.insert((i,j), at: 0)
}else if humScore >= live_Four {
fours.append((i,j))
}else if aiScore >= sleep_Four{
sleepFours.insert((i,j), at: 0)
}else if humScore >= sleep_Four{
sleepFours.append((i,j))
}else if aiScore >= 2*live_Three{
doubleThrees.insert((i,j), at: 0)
}else if humScore >= 2*live_Three{
doubleThrees.append((i,j))
}else if aiScore >= live_Three {
threes.insert((i,j), at: 0)
}else if humScore >= live_Three {
threes.append((i, j))
}else if aiScore >= live_Two{
twos.insert((i,j), at: 0)
}else if humScore >= live_Two{
twos.append((i,j))
}else {
oters.append((i,j))
}
}
}
}

if fives.count > 0 {
return [fives[0]]
}
if fours.count > 0 {
return fours
}
if sleepFours.count > 0{
return [sleepFours[0]]
}
if doubleThrees.count > 0{
return doubleThrees + threes
}
let result = threes + twos + oters
var realy:[(Int,Int)] = []
if result.count > limitNum {
realy += result.prefix(limitNum)
return realy
}
return result
}
```

五子棋是一种进攻优势的棋，依靠连续不断地活三或者冲四进攻，最后很容易会形成必杀棋，所以在进行深度搜索时，我们另开一种连续进攻的搜索，如果，电脑可以依靠连续进攻获得胜利，我们可以直接走这条路劲。这条路劲，其实也是极大值极小值搜索算法的一种，只不过是只考虑活三冲四这两种棋型，指数的底数较小，搜索的节点比较少，因此是效率很高的算法。代码如下：

```swift
//优先考虑ai成五
static func findMaxScore(chessArray:[[FlagType]],role:FlagType,aiScore:[[Int]],humanScore:[[Int]],score:Int)->[(Int,Int,Int)]{
var result:[(Int,Int,Int)] = []
for i in 0..<15{
for j in 0..<15{
if chessArray[i][j] == .freeChess {
if self.effectivePoint(chessArray: chessArray, point: (i,j),chessCount: 1) {
let score1 =  role == .blackChess ?  aiScore[i][j] : humanScore[i][j]
if score1 >= live_Five {
return [(i,j,score1)]
}
if score1 >= score {
result.append((i,j,score1))

}
}
}
}
}
return  result.sorted { (a, b) -> Bool in
return b.2 > a.2
}

}
//考虑活三，冲四
static func findEnemyMaxScore(chessArray:[[FlagType]],role:FlagType,aiScore:[[Int]],humanScore:[[Int]],score:Int)->[(Int,Int,Int)]{
var result:[(Int,Int,Int)] = []
var fours:[(Int,Int,Int)] = []
var fives:[(Int,Int,Int)] = []
for i in 0..<15{
for j in 0..<15{
if chessArray[i][j] == .freeChess {
if  self.effectivePoint(chessArray: chessArray, point: (i,j),chessCount: 1) {
let score1 =  role == .blackChess ?  aiScore[i][j] : humanScore[i][j]
let score2 = role == .blackChess ?  humanScore[i][j] : aiScore[i][j]
if score1 >= live_Five {
return [(i,j,-score1)]
}
if score1 >= live_Four {
fours.insert((i,j,-score1), at: 0)
continue
}
if score2 >= live_Five {
fives.append((i,j,score2))
continue
}
if score2 >= live_Four{
fours.append((i,j,score2))
continue
}
if score1 > score || score2 > score {
result.append((i,j,score1))
}
}
}
}
}
if fives.count > 0 {
return [fives[0]]
}
if fours.count > 0 {
return [fours[0]]
}
return  result.sorted { (a, b) -> Bool in
return abs(b.2) > abs(a.2)
}
}
```
