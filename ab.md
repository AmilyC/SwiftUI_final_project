
<h1>ab</h1>
<h2>1A2B</h2>
<table>
  <tr>
    
    
      
 ```swift
    import SwiftUI

struct GuessRecord: Identifiable{
    var id = UUID()
    var input:String
    var a : Int
    var b : Int
}
struct ab: View {
    @State var isRain = true
    @State private var nextGame = false
    var body: some View {
        
        TabView {
            ViewGame()
                .tabItem{
                    //                                        HStack{
                    //                                            Image(systemName: "cube.box")
                    //                                            Text("1A2B")
                    //                                        }
                }
        }.background(
            NavigationLink(
                destination: ox(), isActive: $nextGame,
                label:{
                    EmptyView()
                }
            ).hidden()
        )
    }
}

struct ViewGame: View {
    @State var n1 = 10
    @State var n2 = 10
    @State var n3 = 10
    @State var n4 = 10
    @State var sum = " "
    @State var ans1 = Int.random(in: 1...9)
    @State var ans2 = 10
    @State var ans3 = 10
    @State var ans4 = 10
    @State var i = 0
    @State var a = 0
    @State var b = 0
    @State var showingAlert = false
    @Environment(\.presentationMode) var presentationMode:Binding<PresentationMode>
    @State private var nextGame = false
    @State var history:[GuessRecord] = []
    //@State var previoisSum = " "
    //輸入數字用的function
    func num(n:Int) -> Void {
        if(i==0){
            i=1
            ini()
        }
        if(n1 == 10){
            n1=n
        }else if(n2==10 && n != n1){
            n2=n
        }else if(n3==10 && n != n2 && n != n1){
            n3=n
        }else if(n4==10 && n != n1 && n != n2 && n != n3){
            n4=n
        }
    }
    //重置答案用的function
    func ini() -> Void {
        ans1 = Int.random(in: 1...9)
        ans2 = Int.random(in: 1...9)
        while ans2 == ans1 {
            ans2 = Int.random(in: 1...9)
        }
        ans3 = Int.random(in: 1...9)
        while ans3 == ans1 || ans3 == ans2 {
            ans3 = Int.random(in: 1...9)
        }
        ans4 = Int.random(in: 1...9)
        while ans4 == ans1 || ans4 == ans2 || ans4 == ans3 {
            ans4 = Int.random(in: 1...9)
        }
        print("正確答案: \(ans1)\(ans2)\(ans3)\(ans4)")
    }
    //1A2B介面
    var body: some View {
        VStack {
               
            HStack {
               
                Image(systemName: "\(a).circle").resizable()
                    .frame(width: 50.0, height: 50.0)
                Image(systemName: "a.square").resizable()
                    .frame(width: 50.0, height: 50.0)
                Image(systemName: "\(b).circle").resizable()
                    .frame(width: 50.0, height: 50.0)
                Image(systemName: "b.square").resizable()
                    .frame(width: 50.0, height: 50.0)
            }
            //            .padding(50)
            //            .padding(.top, 50).padding(.bottom, -20)
            //            Text(sum)
            // 顯示歷史
            ScrollView{
                VStack{ 
                    Text("歷史紀錄")
                        .font(.headline)
                        .padding(.top,20)
                    ForEach(history){record in
                        //ScrollView{
                        HStack{
                            Text("\(record.input),\(record.a)A\(record.b)B")
                                .padding(.bottom,5)
                            Spacer()
                        }
                        // }
                    }
                }
            }
            HStack {
                nimg1(n:$n1)
                nimg1(n:$n2)
                nimg1(n:$n3)
                nimg1(n:$n4)
            }
            .padding(50).padding(.top, -20)
            VStack{
                HStack {
                    Button(action: {
                        self.num(n: 1)
                    }) {
                        Image("1.1")
                            .resizable()
                            .frame(width: 70.0, height: 70.0)
                            .foregroundColor(Color(red:151/255,green:167/255,blue:187/255))
                    }
                    Spacer()
                    Button(action: {
                        self.num(n: 2)
                    }) {
                        Image("2.1")
                            .resizable()
                            .frame(width: 70.0, height: 70.0)
                            .foregroundColor(Color(red:151/255,green:167/255,blue:187/255))
                    }
                    Spacer()
                    Button(action: {
                        self.num(n: 3)
                    }) {
                        Image("3.1")
                            .resizable()
                            .frame(width: 70.0, height: 70.0)
                            .foregroundColor(Color(red:151/255,green:167/255,blue:187/255))
                    }
                }
                .padding(50).padding(.top, -50)
                HStack {
                    Button(action: {
                        self.num(n: 4)
                    }) {
                        Image("4.1")
                            .resizable()
                            .frame(width: 70.0, height: 70.0)
                            .foregroundColor(Color(red:151/255,green:167/255,blue:187/255))
                    }
                    Spacer()
                    Button(action: {
                        self.num(n: 5)
                    }) {
                        Image("5.1")
                            .resizable()
                            .frame(width: 70.0, height: 70.0)
                            .foregroundColor(Color(red:151/255,green:167/255,blue:187/255))
                    }
                    Spacer()
                    Button(action: {
                        self.num(n: 6)
                    }) {
                        Image("6.1")
                            .resizable()
                            .frame(width: 70.0, height: 70.0)
                            .foregroundColor(Color(red:151/255,green:167/255,blue:187/255))
                    }
                }
                .padding(50).padding(.top, -50)
                HStack {
                    Button(action: {
                        self.num(n: 7)
                    }) {
                        Image("7.1")
                            .resizable()
                            .frame(width: 70.0, height: 70.0)
                            .foregroundColor(Color(red:151/255,green:167/255,blue:187/255))
                    }
                    Spacer()
                    Button(action: {
                        self.num(n: 8)
                    }) {
                        Image("8.1")
                            .resizable()
                            .frame(width: 70.0, height: 70.0)
                            .foregroundColor(Color(red:151/255,green:167/255,blue:187/255))
                    }
                    Spacer()
                    Button(action: {
                        self.num(n: 9)
                    }) {
                        Image("9.1")
                            .resizable()
                            .frame(width: 70.0, height: 70.0)
                            .foregroundColor(Color(red:151/255,green:167/255,blue:187/255))
                    }
                }
                .padding(50).padding(.top, -50).padding(.bottom, -50)
            }.background{
                Image("ab_map")
                    .resizable()
                    .frame(width: 1180, height: 780)
//                    .scaledToFill()
//                    .opacity(0.3)
                    .clipped()
                NavigationLink(
                    destination: ox(), // 下一個遊戲的 struct
                    isActive: $nextGame, // 用於觸發導航的變數
                    label: {
                        EmptyView()
                    }
                ).hidden()
            }
            
            
           
                    
            HStack {
                Button(action: {
                    self.ans1 = Int.random(in: 1...9)
                    self.ans2 = 10
                    self.ans3 = 10
                    self.ans4 = 10
                    self.ini()
                    self.sum=" "
                    self.a=0
                    self.b=0
                    //清空
                    self.history = []
                    nextGame = false
                }) {
                    ZStack{
                        Rectangle()
                            .fill(Color(red:142/255,green:77/255,blue:0/255))
                            .frame(width: 100, height: 30)
                            .cornerRadius(10)
                        HStack{
                            Image(systemName: "repeat")
                                .foregroundColor(Color(red: 255/255, green: 224/255, blue: 184/255))
                            Text("開新遊戲")
                                .foregroundColor(Color(red: 255/255, green: 224/255, blue: 184/255))    
                        }    
                    }
                    
                    
                }
                Spacer()
                Button(action: {
                    self.n1=10
                    self.n2=10
                    self.n3=10
                    self.n4=10
                    
                }) {
                    ZStack{
                        Rectangle()
                            .fill(Color(red:142/255,green:77/255,blue:0/255))
                            .frame(width: 100, height: 30)
                            .cornerRadius(10)
                        HStack{
                            Image(systemName: "xmark.octagon")
                                .foregroundColor(Color(red: 255/255, green: 224/255, blue: 184/255))
                            Text("重置數字")
                                .foregroundColor(Color(red: 255/255, green: 224/255, blue: 184/255))    
                        }    
                    }
                    
                    
                }
                Spacer()
                Button(action: {
                    if(self.n4<10){
                        self.a=0
                        self.b=0
                        if(self.n1==self.ans1){
                            self.a+=1
                        }
                        if(self.n2==self.ans2){
                            self.a+=1
                        }
                        if(self.n3==self.ans3){
                            self.a+=1
                        }
                        if(self.n4==self.ans4){
                            self.a+=1
                        }
                        if(self.n1==self.ans2||self.n1==self.ans3||self.n1==self.ans4){
                            self.b+=1
                        }
                        if(self.n2==self.ans1||self.n2==self.ans3||self.n2==self.ans4){
                            self.b+=1
                        }
                        if(self.n3==self.ans2||self.n3==self.ans1||self.n3==self.ans4){
                            self.b+=1
                        }
                        if(self.n4==self.ans2||self.n4==self.ans3||self.n4==self.ans1){
                            self.b+=1
                        }
                        
                        
                        self.sum = "\(self.n1)\(self.n2)\(self.n3)\(self.n4)"
                        
                        //儲存輸入
                        //self.history.append((self.sum,self.a,self.b))
                        self.history.append(GuessRecord(input: self.sum, a: self.a, b: self.b))
                        
                        self.n1=10
                        self.n2=10
                        self.n3=10
                        self.n4=10
                        if(self.a==4){
                            self.showingAlert = true
                            self.ans1 = Int.random(in: 1...9)
                            self.ans2 = 10
                            self.ans3 = 10
                            self.ans4 = 10
                            self.ini()
                            self.sum=" "
                            self.a=0
                            self.b=0
                            
                            // 设置 nextGame 为 true，触发导航
                            //self.nextGame = true
                            //清空
                            //self.history = []
                        }
                    }
                }) {
                    ZStack{
                        Rectangle()
                            .fill(Color(red:142/255,green:77/255,blue:0/255))
                            .frame(width: 100, height: 30)
                            .cornerRadius(10)
                        HStack{
                            Image(systemName: "play.circle")
                                .foregroundColor(Color(red: 255/255, green: 224/255, blue: 184/255))
                            Text("確認答案")
                                .foregroundColor(Color(red: 255/255, green: 224/255, blue: 184/255))
                        }    
                    }
                    
                    
                }
                .alert("恭喜過關🎉", isPresented: $showingAlert) {
                        Button("前往下一個關卡")
                        {
                            //ooxxstory()
                            self.nextGame = true
                        }
                } message: {
                    Text("圈圈叉叉是一場智力對決，你需要在棋盤上巧妙地佈局，贏得這場符號之爭就能獲得寶物")
                }
            }.padding()
            
                    // 這裡新增一個 Text 顯示正確答案
                    Text("正確答案: \(ans1)\(ans2)\(ans3)\(ans4)")
                        .foregroundColor(.blue)
                        .padding(.top, 20)
        }
        .background{
            Image("bombBack")
                .resizable()
                .scaledToFill()
                .opacity(0.3)
                .clipped()
            NavigationLink(
                destination: ox(), // 下一個遊戲的 struct
                isActive: $nextGame, // 用於觸發導航的變數
                label: {
                    EmptyView()
                }
            ).hidden()
        }
        //.padding()
              }
}
struct ViewGame_Previews: PreviewProvider {
    static var previews: some View {
        ViewGame()
    }
}
//顯示輸入的數字
struct nimg1: View {
    @Binding var n:Int
    var body: some View {
        if(n<10){
            return Image(systemName: "\(n)"+".square")
                .resizable()
                .frame(width: 70.0, height: 70.0)
        }
        return Image(systemName: "square")
            .resizable()
            .frame(width: 70.0, height: 70.0)
        
    }
}



    

      
 ```

  <img src="https://raw.githubusercontent.com/AmilyC/Yzu-swiftui/main/Hw1.png">
     </td>
  </tr>
</table>
