 <h1>ooxxstory</h1>
<h2>ooxx</h2>
<table>
  <tr>
    
    
      
 ```swift
  
import SwiftUI
import Foundation

struct ox: View {
    @State var isRain = true
    @State private var nextGame = false
    var body: some View {
        
        TabView {
            ooxxstory()
                .tabItem{
                    //HStack{
                    //   Image(systemName: "cube.box")
                    //   Text("1A2B")
                    //}
                }
        }.background(
            NavigationLink(
                destination: flip(), isActive: $nextGame,
                label:{
                    EmptyView()
                }
            ).hidden()
        )
    }
}

struct ooxxstory: View {
    @State private var nextGame = false
    var body: some View {
        VStack {
            Text("圈圈叉叉")
                .font(.system(size:50))
                .fontWeight(.heavy)
                .foregroundStyle(.primary)
                .background(Color(red:151/255,green: 167/255,blue:187/255))
                .cornerRadius(100)
                .frame(minWidth:0,idealWidth: 400,maxWidth: .infinity,minHeight: 0,idealHeight: 100,maxHeight: 100,alignment: .center)
                .padding()
            //
            SinglePlayerStory()

        }
        .background{
            Image("bombBack")
                .resizable()
                .scaledToFill()
                .opacity(0.3)
                .clipped()
                .aspectRatio(contentMode: .fill)
            NavigationLink(
                destination: flip(), // 下一個遊戲的 struct
                isActive: $nextGame, // 用於觸發導航的變數
                label: {
                    EmptyView()
                }
            ).hidden()
        }
    }
}

struct SinglePlayerStory: View {
    @State private var moves = ["","","","","","","","",""]
    @State private var endGameText = "單人模式"
    @State private var gameEnded = false
    @State private var nextGame = false
    private var ranges = [(0..<3),(3..<6),(6..<9)]
    
    
    var body: some View {
        VStack {
            Spacer()
            Button(action: {
                endGameText = "單人模式"
                moves = ["","","","","","","","",""]
            }, label: {
                Text("Reset")
//                    .foregroundColor(.white)
                    .fontWeight(.bold)
                    .font(.system(size: 45, design: .serif))
//                    .font(.system(size:45))
                    .foregroundColor(Color(red:142/255,green:77/255,blue:0/255))
//                    .fontWeight(.heavy)
//                    .foregroundStyle(.primary)
                    .background(Color(red: 255/255, green: 224/255, blue: 184/255))
                    .cornerRadius(10)
                    .frame(minWidth:0,idealWidth: 400,maxWidth: .infinity,minHeight: 0,idealHeight: 100,maxHeight: 100,alignment: .center)
                    .padding()
            })
            Text("")
                .alert("恭喜過關🎉", isPresented: $gameEnded) {
                    Button("前往下一個關卡")
                    {
                        flip()
                        nextGame = true
                    }
                } message: {
                    Text("你必須在有限的時間內，找出10種相同武器，如果能成功，你將獲得武器之石，否則就要一直被困在這！\n5準備好後就開始吧！")
                }
                .padding(.top,50)
                .font(/*@START_MENU_TOKEN@*/.title/*@END_MENU_TOKEN@*/)
            Spacer()
            ForEach(ranges, id: \.self) { range in
                HStack {
                    ForEach(range, id: \.self) { i in
                        XOButton(letter: $moves[i])
                            .simultaneousGesture(
                                TapGesture()
                                    .onEnded { _ in
                                        Tap(index: i)
                                    }
                            )
                    }
                }
            }
            Spacer()
            .padding(.bottom,70)
            NavigationLink(
                destination: flip(), // 下一個遊戲的 struct
                isActive: $nextGame, // 用於觸發導航的變數
                label: {
                    EmptyView()
                }
            ).hidden()
        }.background{
            Image("ooxx_map")
                .resizable()
                .frame(width: 550, height: 550)
            //                        .scaledToFill()
            //                        .opacity(0.3)
                .clipped()
        }
        
    }
    
    func Tap(index: Int) {
        if moves[index] == "" {
            moves[index] = "X"
            botMove()
        }
        
        for letter in ["X", "O"] {
            if check(list: moves, letter: letter) {
                endGameText = "\(letter) 贏了!"
                gameEnded = true
                break
            }else if checkeq(list: moves){
                endGameText = "平手"
                gameEnded = true     
                break
            }
        }  
    }
    
    func botMove() {
        var availableMoves: [Int] = []
        var movesLeft = 0
        
        for move in moves {
            if move == "" {
                availableMoves.append(movesLeft)
            }
            movesLeft += 1
        }
        
        if availableMoves.count != 0 {
            moves[availableMoves.randomElement()!] = "O"
        }
    }
}




    

      
 ```

  <img src="https://raw.githubusercontent.com/AmilyC/Yzu-swiftui/main/Hw1.png">
     </td>
  </tr>
</table>
