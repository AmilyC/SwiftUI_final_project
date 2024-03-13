
<h1>Bomb</h1>
<h2>數字炸彈 故事模式</h2>
<table>
  <tr>
    
    
      
 ```swift
  
import SwiftUI

struct inBomb: View {
    @State var isRain = true
    @State private var nextGame = false
    var body: some View {
        TabView {
            bomb()
                .tabItem{
                    //HStack{
                    //Image(systemName: "cube.box")
                    //Text("1A2B")
                    //}
                }
        }.background(
            NavigationLink(
                destination:ab(), isActive: $nextGame,
                label:{
                    EmptyView()
                }
            ).hidden()
        )
    }
}

struct bomb: View {
    @State var message = "開始猜吧！"
    @State var guessNum: Int? = 0
    @State var upperBound = 100
    @State var lowerBound = 1
    @State var time = 6
    
    @State var state = true
    @State var guessResult = false
    @State var guessResultlose = false
    @State private var endGameText = ""
    @State private var nextGame = false
    @State private var showAlert = false
    @State var ans = Int.random(in: 1...100)
    var body: some View {
        ZStack(alignment: .bottom){
            VStack{
                // 在這裡加上 Text 顯示答案
                Text("正確答案：\(ans)")
                    .foregroundColor(.blue)
                    .font(.system(size: 20))
                    .padding()
                Text("") 
                    .alert("恭喜過關🎉", isPresented: $guessResult) {
                        Button("前往下一個關卡")
                        {
                            ab()
                            nextGame = true
                        }
                    } message: {
                        Text("接下來，你進入了1A2B的異境，挑戰著更複雜的數字謎題。這裡的每一步都代表著一種轉折，要玩家巧妙地揭示每個數字的秘密，才能獲得智慧之匙！")
                    }
                    
                Text("") 
                    .alert("再接再厲！！", isPresented: $guessResultlose) {
                        Button("重來", role: .destructive)
                        {
                            self.guessNum = 0
                            self.message = "開始猜吧！"
                            self.state = true
                            self.upperBound = 100
                            self.lowerBound = 1
                            self.time = 6
                            
                            self.ans = Int.random(in: 1...100)
                        }
                    }
                Text("數字炸彈")
                    .font(.system(size:50))
                    .frame(minWidth: 0, idealWidth: 400, maxWidth: .infinity, minHeight: 0, idealHeight: 100, maxHeight: 100, alignment: .center)
                    .background(Color(red:151/255,green:167/255,blue:187/255))
                    .foregroundColor(.white)
                    .cornerRadius(100)
                    .padding()
                VStack{
                    HStack{
                        Text("範圍是")
                            .font(.system(size:18))
                        Text("\(lowerBound)~\(upperBound)")
                            .font(.system(size:25))
                            .fontWeight(.heavy)
                            .foregroundColor(.purple)
                        
                        Text("還有")
                            .font(.system(size:18))
                        Text("\(time)")
                            .font(.system(size:25))
                            .fontWeight(.heavy)
                            .foregroundColor(.pink)
                        
                        Text("次")
                            .font(.system(size:18))
                        
                        
                    }
                    Text("請在有限次數內解除炸彈")
                        .font(.system(size:18))
                    //TextField("",value: $guessNum,formatter: NumberFormatter()){
                }
                
                Image("炸彈")
                    .resizable()
                    .frame(width:400,height: 400)
                    .offset(x:50,y:-30)
                    .overlay{
                        
                        TextField("輸入數字", text: Binding(
                            get: { "\(self.guessNum ?? 0)" },
                            set: {
                                if let value = Int($0) {
                                    self.guessNum = value
                                }
                            }
                        )){  
                            if self.state{
                                if(self.guessNum != nil){
                                    if(self.guessNum! >= self.lowerBound)&&(self.guessNum! <=      self.upperBound){
                                        self.time = self.time-1
                                        //self.guessNum = self.guessNum
                                        //self.message = "超過範圍嘍！"
                                        if self.guessNum! > self.ans{
                                            self.upperBound = self.guessNum! - 1
                                            self.message = "猜少一點"
                                            self.guessResult = false
                                        }else if self.guessNum! < self.ans{
                                            self.lowerBound = self.guessNum! + 1
                                            self.message = "猜多一點"
                                            self.guessResult = false
                                        }else{
                                            self.message = "答對了！"
                                            self.state = false
                                            self.guessResult = true
                                        }
                                        
                                        if self.time == 0 && self.guessResult == false{
                                            self.message = "失敗了，超過次數"
                                            self.state = false
                                            self.guessResultlose = true
                                        }
                                    }else{
                                        self.message = "超出範圍喔！"
                                        self.guessResult = false
                                    }
                                }
                            }
                            //print(self.guessNum)
                            
                        }
                        .font(.system(size:50))
                        .frame(width: 100, height: 100)
                        .overlay(RoundedRectangle(cornerRadius: 20).stroke(Color.white, lineWidth: 5))
                        .multilineTextAlignment(.center)
                        .foregroundColor(.white)
                        .keyboardType(.numberPad)
                        .padding()
                    }
                Text(message)
                    .font(.system(size: 25))
                //}
                
                Button(action: {
                    
                    self.guessNum = 0
                    self.message = "開始猜吧！"
                    self.state = true
                    self.upperBound = 100
                    self.lowerBound = 1
                    self.time = 6
                    nextGame = false
                    self.ans = Int.random(in: 1...100)
                    
                    self.showAlert = true
                    
                }) {
                    HStack{
                        Text("重玩")
                            .foregroundColor(.black)
                        Image(systemName: "arrow.uturn.left.square")
                            .resizable()
                            .frame(width: 25, height: 25)
                            .foregroundColor(.black)
//                            .background(Color.white)
                    }
                }
                .offset(x: -150, y: -30)
                .alert(isPresented: $showAlert) { () -> Alert in
                    
                    if guessResult == true {
                        
                        let response = ["太簡單了重來", "我要提高難度了", "你太瞭解我了"]
                        return Alert(title: Text(response.randomElement()!))
                        
                    } else {
                        
                        let response = ["再給你一次機會", "有這麼難猜嗎?", "遜咖", "加油好嗎","好笨","你黃昱儒？","你章魚哥的房子？"]
                        
                        return Alert(title: Text(response.randomElement()!))
                    }   
                }
            }
            .background{
                Image("B_back")
                    .resizable()
                    .frame(width: 800, height: 1700)
                    .rotationEffect(Angle.init(degrees: 90))
                                       
                                        .scaledToFill()
                //                        .opacity(0.3)
//                                        .clipped()
                //                        .aspectRatio(contentMode: .fill)
                
            }
        }
        .background{
                Image("bombBack")
                    .resizable()
                    .scaledToFill()
                    .opacity(0.3)
                    .clipped()
                    .aspectRatio(contentMode: .fill)
            NavigationLink(
                destination: ab(), // 下一個遊戲的 struct
                isActive: $nextGame, // 用於觸發導航的變數
                label: {
                    EmptyView()
                }
            ).hidden()
        }
        
    }
}

//struct ContentView_Previews: PreviewProvider {
//    static var previews: some View {
//        bomb()
//    }
//}




    

      
 ```

  <img src="https://raw.githubusercontent.com/AmilyC/Yzu-swiftui/main/Hw1.png">
     </td>
  </tr>
</table>
