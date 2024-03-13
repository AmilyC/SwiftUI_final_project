<h1>Flipcard</h1>
<h2>翻牌遊戲 故事模式</h2>
<table>
  <tr>
    
    
      
 ```swift
  
import SwiftUI

struct flip: View {
    @State var isRain = true
    @State private var nextGame = false
    var body: some View {
        
        TabView {
            Flipcard()
                .tabItem{
                    //HStack{
                    //Image(systemName: "cube.box")
                    //Text("1A2B")
                    //}
                }
        }.background(
            NavigationLink(
                destination: inCatch(), isActive: $nextGame,
                label:{
                    EmptyView()
                }
            ).hidden()
        )
    }
}

struct Card: Identifiable{
    var id = UUID()
    var image:String
    var flip: Bool
    var nowId: Int
}

struct Suit: Identifiable{
    var id:Int
    var image:String
}

struct Flipcard: View {
    @State var suit = [
        Suit(id: 0, image: "s1"),
        Suit(id:1, image: "s2"),
        Suit(id:2, image: "s3"),
        Suit(id:3, image: "s4"),
        Suit(id:4, image: "s5"),
        Suit(id:5, image: "s6"),
        Suit(id:6, image: "s7"),
        Suit(id:7, image: "s8"),
        Suit(id:8, image: "s9"),
        Suit(id:9, image: "s10"),
    ]
    @State var cards = [
        Card(image: "s1", flip: false, nowId: 0),
        Card(image: "s1", flip: false, nowId: 1),
        Card(image: "s2", flip: false, nowId: 2),
        Card(image: "s2", flip: false, nowId: 3),
        Card(image: "s3", flip: false, nowId: 4),
        Card(image: "s3", flip: false, nowId: 5),
        Card(image: "s4", flip: false, nowId: 6),
        Card(image: "s4", flip: false, nowId: 7),
        Card(image: "s5", flip: false, nowId: 8),
        Card(image: "s5", flip: false, nowId: 9),
        Card(image: "s6", flip: false, nowId: 10),
        Card(image: "s6", flip: false, nowId: 11),
        Card(image: "s7", flip: false, nowId: 12),
        Card(image: "s7", flip: false, nowId: 13),
        Card(image: "s8", flip: false, nowId: 14),
        Card(image: "s8", flip: false, nowId: 15),
        Card(image: "s9", flip: false, nowId: 16),
        Card(image: "s9", flip: false, nowId: 17),
        Card(image: "s10", flip: false, nowId: 18),
        Card(image: "s10", flip: false, nowId: 19),
    ]
    @State var timer: Timer? 
    @State var remain: Int = 70
    @State var all: Int = 0
    @State var counts:Int = 20
    @State var arr = [Int]()
    @State var finish:Bool = false//判斷遊戲是否結束
    
    @State var num:Int = 0
    @State var now:Int = 20
    @State var start:Bool = false
    @State var showAlert:Bool = false
    
    @State var title:String = "時間到！"
    @State var message:String = "闖關失敗"
    @State var dis:String = "再試一次QQ"
    @State var nowRemain:Int = 70
    @State private var nextGame = false
    
    
    var body: some View {
        
        VStack{
            Text("剩餘時間: \(remain) second")
                .padding()
                .background{
                    Image("ab_map")
                        .resizable()
                        .frame(width: 250, height: 100)
                    //                    .scaledToFill()
                    //                    .opacity(0.3)
                        .clipped()
                }
            Button(action: {
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
                all = 0
                
            }, label: {
                Text("START")
                    .padding(.all,10)
                    .frame(width: 180, height: 80, alignment: /*@START_MENU_TOKEN@*/.center/*@END_MENU_TOKEN@*/)
                    .font(.system(size: 45, design: .serif))
                    .background(Color(red: 25/255, green: 74/255, blue: 0/255))
                    .foregroundColor(.white)
                    .cornerRadius(20)
            })
            
            LazyVGrid(columns: Array(repeating: GridItem(), count: 4), spacing: 30) {//自動換行的方法
                ForEach(cards){ card in
                    Button{
                        withAnimation {
                            print(num)
                            if(num == 2){
                                num = 0;
                                now = 20;
                            }
                            if(num == 0){
                                now = card.nowId
                            }
                            num+=1;
                            Flip(tapCard: card)
                        }
                    } label: {
                        CardsView(card: card)
                            .aspectRatio(2/3, contentMode: .fit)
                    }
                }
                
            }
//            .background{
//                Image("ab_map")
//                    .resizable()
//                    .frame(width: 1180, height: 820)
//                                    .scaledToFill()
//                //                    .opacity(0.3)
//                    .clipped()
//            }
            
            .onAppear{
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
                all = 0
                startTime()
//                timer = Timer.scheduledTimer(withTimeInterval: 1.0, repeats: true){ _ in
//                    if all == 20{
//                        stopTimer()
//                    }
//                    else if remain > 0 {
//                        remain -= 1
//                    }
//                    else{
//                        stopTimer()
//                    }
//                }
            }
            .alert(isPresented: $showAlert) {
                Alert(
                    title: Text(title),
                    message: Text(message),
                    dismissButton: .default(Text(dis)) {
                        arr = [Int]()
                        all = 0
                        if finish == true{
                            //finish = false
                            //nextgame
                            nextGame = true 
//                            finish = false
                        }
                        else{
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
                            finish = false
                        }
                        showAlert = false
                        // 可以在按下 OK 按鈕後執行任何操作
                    }
                )
            }
            NavigationLink(
                destination: inCatch(), isActive: $nextGame,
                label:{
                    EmptyView()
                }
            ).hidden()
        }
        
//        .frame(minWidth: 0, maxWidth: .infinity, minHeight: 0, maxHeight: .infinity)
      
        
        .background{
            Image("bombBack")
                .resizable()
                .scaledToFill()
                .opacity(0.3)
        }
    }
    
    func Flip(tapCard: Card){
        cards[tapCard.nowId].flip = true
        if(num == 2){
            if (cards[now].image == cards[tapCard.nowId].image) {
                print("Correct match!")
                all += 2
                for num in 0..<(arr.count){
                    if(cards[num].flip == false){
                        
                        print(all)
                        break
                    }
                    else if(num == arr.count-1){
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
                //remain = 70
            }
        }
    }
    
    func stopTimer(){
        timer?.invalidate()
        timer = nil
        
        if(remain > 0 && remain < nowRemain){
            title = "恭喜破關🎉"
            message = "接著，你必須在有限的時間內接住一定數量的紅蘿蔔才能通關。通過了這場速度和反應的挑戰就能獲得速度之石 "
            dis = "前往下一個關卡"
            finish = true
            // remain = nowRemain
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

struct CardsView: View {
    let card: Card
    
    var body: some View {
        ZStack {
            
            
            Image(card.image)
                .resizable()
                .aspectRatio(contentMode: /*@START_MENU_TOKEN@*/.fill/*@END_MENU_TOKEN@*/)
                .foregroundColor(.black)
                .frame(width: 60, height: 60)
                //.opacity(card.flip ? 1.0 : 0.0)
            
            Image("Unknow")
                .resizable()
                .aspectRatio(contentMode: .fill) 
                .frame(width: 60, height: 60)
                .opacity(card.flip ? 1.0 : 0.0)
        }
        .rotation3DEffect(
            .degrees(card.flip ? 180 : 0),
            axis: (x: 0.0, y: 1.0, z: 0.0)
        )
    }
}



    

      
 ```

  <img src="https://raw.githubusercontent.com/AmilyC/SwiftUI_final_project/main/IMG_0567.png">
     </td>
  </tr>
</table>
