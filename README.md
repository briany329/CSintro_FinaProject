# CSintro_FinaProject
Sokoban

與 芒果貓 的code相比，大致上修改的地方為:

1. 新增關卡，亦可再新增
Main.jack內以game1~3 分別代表第1~3關
Main依序呼叫1~3關
舉例:let game1 = Game.new(1, 4, 4);
Game.new後面第一個參數為level /**第一關為1，第二關為2，以此類推，把level當參數是為了方便判斷，
                                才不用重複寫數個function判斷不同關卡的同件事情**/
Game.new會呼叫Player.new，除了坐標外，也把level當參數傳入了

接著呼叫Map.map1();
class Map內每張地圖都是一個function，方便class Main呼叫
在funtion map1裡面宣告了一個陣列存數個destination，也有順便把數量存在int num_dest內
同時在這function內會呼叫class Draw底下的函式Draw.drawMap1
class Draw底下寫了各種函式，除了基本的Box、Wall、Player外，也把地圖刻上去了     //可新增地圖
/**注意 : Box跟Wall原本是各做成一個物件，但實際上在這做法裡面用不到這兩個物件的功能，
          於是改成直接畫上去而已，只是圖形，至於destination其實似乎也行，不過不確定**/

2. 建好reset與judge 
按R會reset當前關卡，按J會判斷是否成功，若為是則進到下一關，Q鍵暫時被我註解掉了
reset與judge都寫在class Game底下，
reset做法為先將畫面清空，再依照level重新呼叫Map.map1()，而player直接construct新的
judge做法為依照level去看dest的位置是否被Box蓋過

3. 修改人無法走到destination上面以及destination消失問題
關於人無法移動到destination上面原因是movecheck原先並未判斷到人的下一格是dest的情況，所以最後會return 0
故新增
        // there is a dest int the next space
        if(Memory.peek(next) = 384){
            return -1;
        }
回傳值用-1是因為Player在移動時，若是movecheck回傳值>1，則會在next2畫箱子，再把人移動到next
這部分回傳值跟Player移動時判斷的回傳值大小可以再改，我只是懶得改

destination被覆蓋後就消失的問題，
我的解決方法是在Player每次移動時，都去檢查(recover、check_recover)每個dest的位置是否為0(清空)，
若為0則在該處再畫上destination

雖然每次移動時都會跑一次檢查，不過dest數量並不多，加上移動速度不會太快，所以速度上應該影響不大

覺得還能修改的地方:
code美化，有些地方寫得不優
關卡可以考慮增加，如果都是這種小地圖的話可以考慮放大
