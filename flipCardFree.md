
 <h1>flipCardFree</h1>
<h2>翻牌遊戲 自由模式</h2>
<table>
  <tr>
    
    
      
 ```swift
  

import SwiftUI

struct Cardfree: Identifiable{ //每張Card
    var id = UUID()
    var image:String
    var flip: Bool
    var nowId: Int
}

struct Suitfree: Identifiable{//花色
    var id:Int
    var image:String
}

struct flipCardFree: View {
    @State var suit = [
        Suitfree(id: 0, image: "joker"),
        Suitfree(id:1, image: "b1"),
        Suitfree(id:2, image: "bK"),
        Suitfree(id:3, image: "bQ"),
        Suitfree(id:4, image: "g3"),
        Suitfree(id:5, image: "o7"),
        Suitfree(id:6, image: "oK"),
        Suitfree(id:7, image: "p5"),
        Suitfree(id:8, image: "pK"),
        Suitfree(id:9, image: "gJ"),
    ]
    @State var cards = [
        Cardfree(image: "joker", flip: false, nowId: 0),
        Cardfree(image: "b1", flip: false, nowId: 1),
        Cardfree(image: "joker", flip: false, nowId: 2),
        Cardfree(image: "b1", flip: false, nowId: 3),
        Cardfree(image: "bK", flip: false, nowId: 4),
        Cardfree(image: "bK", flip: false, nowId: 5),
        Cardfree(image: "bQ", flip: false, nowId: 6),
        Cardfree(image: "bQ", flip: false, nowId: 7),
        Cardfree(image: "g3", flip: false, nowId: 8),
        Cardfree(image: "g3", flip: false, nowId: 9),
        Cardfree(image: "gJ", flip: false, nowId: 10),
        Cardfree(image: "gJ", flip: false, nowId: 11),
        Cardfree(image: "o7", flip: false, nowId: 12),
        Cardfree(image: "o7", flip: false, nowId: 13),
        Cardfree(image: "oK", flip: false, nowId: 14),
        Cardfree(image: "oK", flip: false, nowId: 15),
        Cardfree(image: "p5", flip: false, nowId: 16),
        Cardfree(image: "p5", flip: false, nowId: 17),
        Cardfree(image: "pK", flip: false, nowId: 18),
        Cardfree(image: "pK", flip: false, nowId: 19),
    ]
    @State var timer: Timer?
    @State var remain: Int = 70 //初始時間
    @State var all: Int = 0 //紀錄正確數量
    @State var counts:Int = 20 //紀錄有幾張是正確的
    @State var arr = [Int]() //存放花色的array
    
    
    @State var num:Int = 0 //紀錄有幾張牌被翻過來
    @State var now:Int = 20
    @State var start:Bool = false
    @State var showAlert:Bool = false 
    
    @State var title:String = "時間到！🥺"
    @State var message:String = "倒數計時已結束。"
    @State var dis:String = "重玩QQ"
    @State var nowRemain:Int = 70 //每次通關時間
     @AppStorage("trendNum") var trendNum = [0,0,0,0,0,0,0,0]
    
    
    var body: some View {
        
        VStack{
            HStack{//顯示倒數計時
                Text("剩餘時間:")
                    .padding()   
                Text("\(remain)")
                    .padding()
                    .font(.system(size: 35, design: .serif))
                    .fontWeight(.bold)
                    .foregroundColor(Color(red: 219/255, green: 89/255, blue: 66/255))
                //                    .cornerRadius(20)
                Text("second")
                    .padding()
            }
            
            //START
            Button(action: {
                arr = [Int]()
                all = 0 //正確數量歸零
                remain = nowRemain //更新現在關卡的剩餘時間
                startTime()
                
                while arr.count < counts { //隨機存放arr中的id
                    let randomNumber = Int.random(in: 0...19)
                    if !arr.contains(randomNumber) { 
                        arr.append(randomNumber)
                    }
                }
                for i in 0..<(arr.count){//用餘數的方式存放圖片到card中
                    cards[i].image = suit[arr[i]%10].image
                    cards[i].flip = false
                }
                
                showAlert = false
                
            }, label: {
                Text("START")
                    .padding(.all,10)
                    .frame(width: 150, height: 80, alignment: /*@START_MENU_TOKEN@*/.center/*@END_MENU_TOKEN@*/)
                    .font(.system(size: 35,design: .serif))
                    .background(LinearGradient(gradient: Gradient(colors: [Color(red: 174/255, green: 214/255, blue: 241/255), Color(red: 248/255, green: 196/255, blue: 113/255)]), startPoint: .leading, endPoint: .trailing))
                    .foregroundColor(.white)
                    .border(LinearGradient(gradient: Gradient(colors: [Color(red: 174/255, green: 214/255, blue: 241/255), Color(red: 248/255, green: 196/255, blue: 113/255)]), startPoint: .leading, endPoint: .trailing), width: 1)
                    .cornerRadius(20)
            })
            
            LazyVGrid(columns: Array(repeating: GridItem(), count: 4), spacing: 30) {//讓牌可以自換行
                ForEach(cards){ card in
                    Button{
                        withAnimation {
                            //                            print(num)
                            if(num == 2){ //當有兩張牌檢查完後，num會設定成0
                                num = 0;
                                now = 20;//回到初始值
                            }
                            if(num == 0){//如果是翻開的第一張，就先把card的id存到now中
                                now = card.nowId
                            }
                            num+=1;
                            Flip(tapCard: card)//進到FlipFree裡面檢查
                        }
                    } label: {
                        CardViewFree(card: card)
                            .aspectRatio(2/3, contentMode: .fit)    
                    }
                }
            }
            .onAppear{//點進來就倒數計時
                arr = [Int]()
                all = 0
                remain = 70
                startTime()
                
                while arr.count < counts {
                    let randomNumber = Int.random(in: 0...19)
                    if !arr.contains(randomNumber) {
                        arr.append(randomNumber)
                    }
                }
                for i in 0..<(arr.count){
                    cards[i].image = suit[arr[i]%10].image
                    cards[i].flip = false
                }
                
                showAlert = false
                startTime()
            }
        }
        .onAppear{
            trendNum[1] = trendNum[1] + 1
        }
        .alert(isPresented: $showAlert) {
            Alert(
                title: Text(title),
                message: Text(message),
                dismissButton: .default(Text(dis)) {
                    arr = [Int]()
                    all = 0
                    startTime()
                    
                    while arr.count < counts {
                        let randomNumber = Int.random(in: 0...19)
                        if !arr.contains(randomNumber) {
                            arr.append(randomNumber)
                        }
                    }
                    for i in 0..<(arr.count){
                        cards[i].image = suit[arr[i]%10].image
                        cards[i].flip = false
                    }
                    
                    showAlert = false
                }
            )
        }
        .frame(minWidth: 0, maxWidth: .infinity, minHeight: 0, maxHeight: .infinity)
        .background(Color(red: 215/255, green: 240/255, blue: 187/255))

    }
    
    func Flip(tapCard: Cardfree){
        cards[tapCard.nowId].flip = true
        if(num == 2){//如果翻開兩張牌
            if (cards[now].image == cards[tapCard.nowId].image) {//檢查牌面是否相同
                print("Correct match!")
                all += 2
                for num in 0..<(arr.count){
                    if(cards[num].flip == false){
                        print(all)
                        break
                    }
                    else if(num == arr.count-1){//如果全部結束後
                        stopTimer()
                    }
                }
            } else {
                print("Incorrect match!")
                
                DispatchQueue.main.asyncAfter(deadline: .now() + 1.0) {
                    withAnimation {
                        cards[tapCard.nowId].flip = false
                        cards[now].flip = false
                    }
                }
            }   
        }
    }
    
    func startTime(){
        timer?.invalidate()
        timer = nil
        timer = Timer.scheduledTimer(withTimeInterval: 1.0, repeats: true){ _ in
            if remain > 0 {
                remain -= 1
            }
            else{
                stopTimer()
                //                remain = 70
            }
        }
    }
    
    func stopTimer(){
        timer?.invalidate()
        timer = nil
        
        if(remain > 0 && remain < nowRemain){
            title = "恭喜破關🎉"
            message = "太強了吧 減10秒繼續挑戰看看吧😏"
            dis = "沒問題！開始"
            nowRemain -= 10
            remain = nowRemain
        }
        else{
            title = "時間到！"
            message = "倒數計時已結束。"
            dis = "重玩QQ"
            remain = nowRemain
        }
        showAlert = true
    }
}

struct CardViewFree: View {
    let card: Cardfree
    
    var body: some View {
        ZStack {
            Image("back")
                .resizable()
                .aspectRatio(contentMode: /*@START_MENU_TOKEN@*/.fill/*@END_MENU_TOKEN@*/) 
                .frame(width: 60, height: 60)
            Image(card.image)
                .resizable()
                .aspectRatio(contentMode: /*@START_MENU_TOKEN@*/.fill/*@END_MENU_TOKEN@*/)
                .foregroundColor(.black)
                .opacity(card.flip ? 1.0 : 0.0)
                .frame(width: 60, height: 60)
        }
        .rotation3DEffect(
            .degrees(card.flip ? 180 : 0),
            axis: (x: 0.0, y: 1.0, z: 0.0)
        )
    }
}


    

      
 ```

  <img src="flipCardFree.png">
     </td>
  </tr>
</table>
