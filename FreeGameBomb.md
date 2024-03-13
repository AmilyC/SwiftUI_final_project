<h1>FreeGameBomb</h1>
<h2>數字炸彈 自由模式</h2>
<table>
  <tr>
    
    
      
 ```swift
   ```swift
import SwiftUI

struct FreeGameBomb: View {
    @State var message = "開始猜吧！"
    @State var guessNum: Int? = 0
    @State var upperBound = 100
    @State var lowerBound = 1
    @State var time = 6
    
    @State var state = true
    @State var guessResult = false
    
    @State private var showAlert = false
    @State var ans = Int.random(in: 1...100)
     @AppStorage("trendNum") var trendNum = [0,0,0,0,0,0,0,0]
    var body: some View {
        ZStack(alignment: .bottom){
            VStack{
                Text("數字炸彈")
                    .font(.system(size:50))
                    .frame(minWidth: 0, idealWidth: 400, maxWidth: .infinity, minHeight: 0, idealHeight: 100, maxHeight: 100, alignment: .center)
                    .background(Color(red:151/255,green:167/255,blue:187/255))
                    .foregroundColor(.white)
                    .cornerRadius(100)
                    .padding()
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
                                            self.guessResult = false
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
                    
                    self.ans = Int.random(in: 1...100)
                    
                    self.showAlert = true
                    
                }) {
                    VStack{
                        Text("重玩")
                            .foregroundColor(.black)
                        Image(systemName: "arrow.uturn.left.square")
                            .resizable()
                            .frame(width: 25, height: 25)
                            .foregroundColor(.black)
                            .background(Color.white)
                    }
                }
                .offset(x: -150, y: 0)
                .alert(isPresented: $showAlert) { () -> Alert in
                    
                    if guessResult == true {
                        
                        let response = ["太簡單了重來", "我要提高難度了", "你太瞭解我了"]
                        return Alert(title: Text(response.randomElement()!))
                        
                    } else {
                        
                        let response = ["再給你一次機會", "有這麼難猜嗎?", "遜咖", "加油好嗎","好笨","你黃昱儒？"]
                        
                        return Alert(title: Text(response.randomElement()!))
                    }   
                }
            }
        }
        .onAppear{
            trendNum[3] = trendNum[3]+1
          //  games[3].trendNum+=1
        }
        .background{
            Image("背景")
                .resizable()
                .scaledToFill()
                .opacity(0.3)
                .clipped()
        }
    }
}
struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}


    

      
 ```

  <img src="https://raw.githubusercontent.com/AmilyC/SwiftUI_final_project/main/bombfree.png">
     </td>
  </tr>
</table>
