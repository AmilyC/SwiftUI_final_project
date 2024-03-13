 <h1>FreeGameCarrot</h1>
<h2>接蘿蔔 自由模式</h2>
<table>
  <tr>
    
    
      
 ```swift
  
import SwiftUI

struct FreeGameCarrot: View {
    @State private var fruits: [Fruit] = []
    @State private var score = 0
    @State private var timer: Timer?
    @State private var timeRemaining = 30.0
    @State private var isGameRunning = false
    @State private var timeInterval = 0.25
     @AppStorage("trendNum") var trendNum = [0,0,0,0,0,0,0,0]
    var body: some View {
        VStack {
            HStack{
                Text("Score: \(score)")
                    .font(.title)
                    .padding(5)
                
                //.background(Color.white)
                
                Button(action: {
                    if isGameRunning {
                        stopGame()
                    } else {
                        startGame()
                    }
                }) {
                    Text(isGameRunning ? "Stop" : "Start")
                        .font(.title)
                        .padding(4)
                        .background(Color.black)
                        .foregroundColor(.white)
                        .border(Color.black,width:4)
                        .cornerRadius(5)
                }
                .padding(.top,5)
            }
            
            ZStack {
                ForEach(fruits) { fruit in
                    Image(systemName: "carrot")
                        .resizable()
                        .foregroundColor(.orange)
                        .frame(width: 50, height: 50)
                        .position(fruit.position)
                        .onTapGesture {
                            removeFruit(fruit)
                        }
                }
            }
            //.border(Color.cyan, width:5)
            //.cornerRadius(5)
            
            
            
            //.padding()
        }
        .onAppear{
            trendNum[7] = trendNum[7]+1
           // games[7].trendNum+=1
        }
    }
    
    func startGame() {
        isGameRunning = true
        score = 0
        timeRemaining = 30
        timer = Timer.scheduledTimer(withTimeInterval: timeInterval, repeats: true) { _ in
            if timeRemaining > 0 {
                spawnFruit()
                timeRemaining -= timeInterval
            } else {
                stopGame()
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

//struct Fruit: Identifiable, Equatable {
//    var id = UUID()
//    var type: FruitType
//    var position: CGPoint
//}

//enum FruitType: String, CaseIterable {
//    case apple = "apple"
//    case banana = "banana"
//    case orange = "orange"
//    // Add more fruit types as needed
//}




    

      
 ```

  <img src="https://raw.githubusercontent.com/AmilyC/SwiftUI_final_project/main/FreeGameCarrot.png">
     </td>
  </tr>
</table>
