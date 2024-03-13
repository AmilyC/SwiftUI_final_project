<h1>catchFruit</h1>
<h2>接蘿蔔 故事模式</h2>
<table>
  <tr>
    
    
      
 ```swift
  

import SwiftUI

struct inCatch: View {
    @State var isRain = true
    @State private var nextGame = false
    var body: some View {
        
        TabView {
            catchFruitView()
                .tabItem{
                    //HStack{
                    //Image(systemName: "cube.box")
                    //Text("1A2B")
                    //}
                }
        }.background(
            NavigationLink(
                destination: inSnake(), isActive: $nextGame,
                label:{
                    EmptyView()
                }
            ).hidden()
        )
    }
}

struct catchFruitView: View {
    @State private var fruits: [Fruit] = []
    @State private var score = 0
    @State private var timer: Timer?
    @State private var timeRemaining = 30.0
    @State private var isGameRunning = false
    @State private var timeInterval = 0.25
    @State private var nextGame = false
    @State private var GameClear = false
    var body: some View {
        VStack {
            HStack{
                Text("Score: \(score)")
//                    .font(.title)
                    .font(.system(.title, design: .serif))
                    .padding(5)
                    .background{
                        Image("ab_map")
                            .resizable()
                            .frame(width: 140, height: 65)
                        //                    .scaledToFill()
                        //                    .opacity(0.3)
                            .clipped()
                    }
                Text("") 
                    .alert("恭喜過關🎉", isPresented: $GameClear) {
                        Button("前往下一個關卡")
                        {
                            nextGame = true
                        }
                    } message: {
                        Text("貪吃蛇的境地是一片夢幻的地形，你必須追逐著奇幻的果實，每一口都將他們帶入不同的冒險場景。\n蛇王說操控小蛇吃到一定長度就能獲得天蛇之瞳。\n你必須連續吃10顆果實，並且不碰到自己和邊界，才能順利破關！")
                    }
                //.background(Color.white)
                Button(action: {
                    if isGameRunning {
                        stopGame()
                    } else {
                        startGame()
                    }
                }) {
                    Text(isGameRunning ? "Stop" : "Start")
//                        .font(.title)
                        .font(.system(.title, design: .serif))
                        .padding(4)
                        .background(Color(red: 25/255, green: 74/255, blue: 0/255))
                        .foregroundColor(.white)
//                        .border(Color.black,width:4)
                        
                        .cornerRadius(5)
                }
                .padding(.top,5)
            }
             
            
            ZStack {
                ForEach(fruits) { fruit in
                    Image("9")
                        .resizable()
                    //.foregroundColor(.green)
                        .frame(width: 50, height: 50)
                        .position(fruit.position)
                        .onTapGesture {
                            removeFruit(fruit)
                        }
                }
            }
            //.border(Color.cyan, width:5)
            //.cornerRadius(5)
            NavigationLink(
                destination: inSnake(), isActive: $nextGame,
                label:{
                    EmptyView()
                }
            ).hidden()
            
            //.padding()
        }
        .frame(minWidth: 0, maxWidth: .infinity, minHeight: 0, maxHeight: .infinity)
        .background{
            Image("bombBack")
                .resizable()
                .scaledToFill()
                .opacity(0.3)
                .clipped()
            NavigationLink(
                destination: inSnake(), isActive: $nextGame,
                label:{
                    EmptyView()
                }
            ).hidden() 
        }
    }
    
    func startGame() {
        isGameRunning = true
        nextGame = false
        score = 0
        timeRemaining = 30
        timer = Timer.scheduledTimer(withTimeInterval: timeInterval, repeats: true) { _ in
            if timeRemaining > 0 {
                spawnFruit()
                timeRemaining -= timeInterval
            } else {
                stopGame()
            }
            
            if score == 30{
                stopGame()
                GameClear = true
            }
            
        }
    }
    
    func stopGame() {
        isGameRunning = false
        timer?.invalidate()
        timer = nil
        fruits.removeAll()
    }
    
    func spawnFruit() {
        let randomX = CGFloat.random(in: 30..<UIScreen.main.bounds.width-30)
        let fruit = Fruit(type: FruitType.allCases.randomElement()!, position: CGPoint(x: randomX, y: 0))
        fruits.append(fruit)
        
        withAnimation(.linear(duration: 2.0)) {
            fruits[fruits.count - 1].position = CGPoint(x: randomX, y: UIScreen.main.bounds.height)
        }
    }
    
    func removeFruit(_ fruit: Fruit) {
        if let index = fruits.firstIndex(of: fruit) {
            fruits.remove(at: index)
            score += 1
        }
    }
}

struct Fruit: Identifiable, Equatable {
    var id = UUID()
    var type: FruitType
    var position: CGPoint
}

enum FruitType: String, CaseIterable {
    case apple = "apple"
    case banana = "banana"
    case orange = "orange"
    // Add more fruit types as needed
}


    

      
 ```

  <img src="https://raw.githubusercontent.com/AmilyC/Yzu-swiftui/main/Hw1.png">
     </td>
  </tr>
</table>
