 <h1>FreeGameFruit</h1>
<h2>水果消消樂 自由模式</h2>
<table>
  <tr>
    
    
      
 ```swift
  


import SwiftUI
//let gamerow = 12
//let gamecol = 9

//var fruitBlocks = Array(repeating: Array(repeating: fruits[0], count: gamecol), count: gamerow)
//var viewStateFruits = Array(repeating: Array(repeating: CGSize.zero, count: gamecol), count: gamerow)
struct FreeGameFruit: View {
    @State var borderColor = Color.black
    @State var draggedFruit:fruit?
    @State var targetRow: Int?
    @State var targetCol:Int?
    @State var fruitBlocks = Array(repeating: Array(repeating: fruitsFree[0], count: gamecol), count: gamerow)
    @State  var viewStateFruits = Array(repeating: Array(repeating: CGSize.zero, count: gamecol), count: gamerow)
    @State var disappearGrid :[GRID] = []
    @State var hintGrid :[GRID] = []
    @State var chooseHint: Int = 0
    @State var haveHint: Bool = false
    //Time
    @State var timer: Timer?
    @State var elapsedTime: Int = 0
    @State var secondElapse: Int = 0
    @State var timeUP: Bool = false
    @State var startDate: Date?
    @State var noActTime: Int = 0
    @State var timerInterval = 1
    @State var  timeUpSecond: Int = 60
    
    //score
    @State var score:Int = 0
    @State var combo :Int = 0
    
    //alert
    @State var showAlert:Bool = false
    @State var conShowAlert = true
     @AppStorage("trendNum") var trendNum = [0,0,0,0,0,0,0,0]
    func initialGame(){
        timeUP = false;
        score = 0
        combo = 0
        elapsedTime = 0
        secondElapse = 0
        noActTime = 0
        generateRandomFruits()
    }
    
    func generateRandomFruits() {
        for row in 0...gamerow-1{
            DispatchQueue.main.asyncAfter(deadline: .now()+0.1*Double(row)){
                for col in 0...gamecol-1{
                    fruitBlocks[row][col] = fruitsFree[Int.random(in: 0..<12)]
                }
            }
            
        }
        DispatchQueue.main.asyncAfter(deadline: .now() + 0.5){
            combo = 0
            disappearG()
        }
    }
    func doSwipe(row:Int ,col: Int){
        let fruitValue = fruitBlocks[row][col]
        // print(fruitBlocks[row][col].direction)
        switch fruitBlocks[row][col].direction{
        case .right:
            // print("case right")
            withAnimation{
                // swap(fruitBlocks[row][col], fruitBlocks[row][col+1])
                fruitBlocks[row][col] = fruitBlocks[row][col+1]
                fruitBlocks[row][col+1] = fruitValue
                viewStateFruits[row][col+1] = .zero
            }
            
            //fruitBlocks[row][col+1]
        case .left:
            fruitBlocks[row][col] = fruitBlocks[row][col-1]
            fruitBlocks[row][col-1] = fruitValue
            viewStateFruits[row][col-1] = .zero
        case .up:
            fruitBlocks[row][col] = fruitBlocks[row-1][col]
            fruitBlocks[row-1][col] = fruitValue
            viewStateFruits[row-1][col] = .zero
        case .down:
            fruitBlocks[row][col] = fruitBlocks[row+1][col]
            fruitBlocks[row+1][col] = fruitValue
            viewStateFruits[row+1][col] = .zero
        default: break
            
        }
        fruitBlocks[row][col].direction = .none
        viewStateFruits[row][col] = .zero
        combo=1
        if haveDisappear(){
            disappearG()
        }
        
    }
    func haveThreeSame(row: Int, col: Int ,value: String, direction: Direction)->Bool{
        //if picture =="" on the row,col four direction will have //line or not
        if value == ""{ return false }
        var right = col
        var left = col
        while right < gamecol-1 && fruitBlocks[row][right+1].image == value && direction != .left{ //check right
            right+=1
        }
        while left > 0 && fruitBlocks[row][left-1].image == value && direction != .right{ //check right
            left-=1
        }
        if right-left >= 2 {
            return true
        }
        var up = row
        var down = row
        while up > 0 && fruitBlocks[up-1][col].image == value && direction != .down{ //check right
            up-=1
        }
        while down < gamerow-1 && fruitBlocks[down+1][col].image == value && direction != .up{ //check right
            down+=1
        }
        if down-up >= 2{
            return true
        }
        return false
        
    }
    func canSwipe(row:Int ,col:Int) ->Bool{
        switch fruitBlocks[row][col].direction{
        case .right:
            if haveThreeSame(row: row, col: col, value: fruitBlocks[row][col+1].image, direction: .left)||haveThreeSame(row: row, col: col+1, value: fruitBlocks[row][col].image, direction: .right){
                return true
                //原本右邊的往左移，原本左邊的往右移
            }
        case .left:
            if haveThreeSame(row: row, col: col, value: fruitBlocks[row][col-1].image, direction: .right)||haveThreeSame(row: row, col: col-1, value: fruitBlocks[row][col].image, direction: .left){
                return true
                //原本右邊的往左移，原本左邊的往右移
            }
        case .up:
            if haveThreeSame(row: row, col: col, value: fruitBlocks[row-1][col].image, direction: .down) || haveThreeSame(row: row-1, col: col, value: fruitBlocks[row][col].image, direction: .up){
                return true
            }
        case .down:
            if haveThreeSame(row: row, col: col, value: fruitBlocks[row+1][col].image, direction: .up) || haveThreeSame(row: row+1, col: col, value: fruitBlocks[row][col].image, direction: .down){
                return true
            }
        default:
            return false
        }
        return false
    }
    func haveDisappear() -> Bool{
        disappearGrid = []
        for re_row in 0..<gamerow{
            let row = gamerow - re_row - 1
            for col in 0..<gamecol{
                if haveThreeSame(row: row, col: col, value: fruitBlocks[row][col].image, direction: fruitBlocks[row][col].direction){
                    disappearGrid.append(GRID(r:row, c: col))
                }
            }
        }
        if disappearGrid.count > 0 { return true }
        else { return false }
    }
    func disappearG(){
        var addScore = 0
        DispatchQueue.main.asyncAfter(deadline: .now()+0.2){
            for grid in disappearGrid{
                fruitBlocks[grid.row][grid.col].image = ""
            }
        }
        DispatchQueue.main.asyncAfter(deadline: .now()+0.3){
            dropDown()
        }
        
        addScore = (disappearGrid.count)*combo
        for i in 0..<addScore{
            DispatchQueue.main.asyncAfter(deadline: .now()+0.1*Double(i)){
                self.score += 1
            }
        }
        if haveHint{
            disappearHint()
        }
        noActTime = 0
    }
    func dropDown(){// 填滿消去的格子
        for col in 0..<gamecol{
            var dropCount = 0
            for re_row in 0..<gamerow{
                let row = gamerow - re_row - 1 //from down to up //check the amount of blanks
                if dropCount != 0 && fruitBlocks[row][col].image != "" {
                    //下面空格由上面物品代替
                    fruitBlocks[row + dropCount][col].image = fruitBlocks[row][col].image
                    viewStateFruits[row + dropCount][col].height = CGFloat(-dropCount * 40)
                    if row - dropCount >= 0{
                        fruitBlocks[row][col].image = fruitBlocks[row-dropCount][col].image
                    }else{
                        fruitBlocks[row][col].image = fruitsFree[Int.random(in: 0..<12)].image
                    }
                    viewStateFruits[row][col].height = CGFloat(-dropCount * 40)
                }
                if fruitBlocks[row][col].image == ""{
                    dropCount += 1
                }
                
            }
            for row in 0..<gamerow{
                if fruitBlocks[row][col].image == ""{
                    fruitBlocks[row][col].image = fruitsFree[Int.random(in: 0..<12)].image
                    viewStateFruits[row][col].height = CGFloat(-dropCount * 40) 
                }
                withAnimation(.linear){
                    viewStateFruits[row][col].height = 0
                }
            }
        }// for col end
        DispatchQueue.main.asyncAfter(deadline: .now() + 0.5){
            if haveDisappear(){
                combo+=1
                disappearG()
            }
        }
    }
    func getHint() ->Bool{
        hintGrid = []
        for r in 0..<gamerow{
            for c in 0..<gamecol{
                if(r+c)%2 == 0{
                    if c < gamecol-1{ //->
                        fruitBlocks[r][c].direction = Direction.right
                        if canSwipe(row: r, col: c){
                            hintGrid.append(GRID(r: r, c: c))
                            hintGrid.append(GRID(r: r, c: c+1))
                        }
                    }
                    if c > 0{ //<-
                        fruitBlocks[r][c].direction = Direction.left
                        if canSwipe(row: r, col: c){
                            hintGrid.append(GRID(r: r, c: c))
                            hintGrid.append(GRID(r: r, c: c-1))
                        }
                    }
                    if r > 0{
                        fruitBlocks[r][c].direction = Direction.up
                        if canSwipe(row: r, col: c){
                            hintGrid.append(GRID(r: r, c: c))
                            hintGrid.append(GRID(r: r-1, c: c))
                        }
                    }
                    if r < gamerow-1{
                        fruitBlocks[r][c].direction = Direction.down
                        if canSwipe(row: r, col: c){
                            hintGrid.append(GRID(r: r, c: c))
                            hintGrid.append(GRID(r: r+1, c: c))
                        }
                    }
                    fruitBlocks[r][c].direction = .none
                }
            }//for col end
        }//for row end
        let hintCount = hintGrid.count/2
        if hintCount == 0{
            generateRandomFruits()
            return false
        }else{
            chooseHint = Int.random(in: 0..<hintCount)
            let g1 = hintGrid[chooseHint*2]
            let g2 = hintGrid[chooseHint*2+1]
            fruitBlocks[g1.row][g1.col].isHint = true
            fruitBlocks[g2.row][g2.col].isHint = true
            return true
        }
        //return true
    }
    func disappearHint(){
        let grid1 = hintGrid[chooseHint*2]
        let grid2 = hintGrid[chooseHint*2+1]
        fruitBlocks[grid1.row][grid1.col].isHint = false
        fruitBlocks[grid2.row][grid2.col].isHint = false
    }
    
    func stopTimer(){
        timer?.invalidate()
        timeUpSecond = 60
        timer = nil
        timeUP = true
//        if score > 50{
//            showAlert = true
//        }
    }
    func startTimer(){
        elapsedTime += secondElapse
        secondElapse = 0
        startDate = Date()
        timeUP = false
        timer = Timer.scheduledTimer(withTimeInterval: 1.0, repeats: true){
            timer in
            if timeUpSecond > 0{
                timeUpSecond -= 1
                noActTime += 1 
                if(noActTime==5){
                    haveHint = getHint()
                    noActTime = 0
                }
            }else{
                stopTimer()
                if haveHint{
                    disappearHint()
                }
            }
            //if let self = self,
            //            let startDate = self.startDate{
            //                
            //            }
        }
    }
    var body: some View {
        // 新頁面的內容
        VStack{
            HStack{
                
                Text("Timer:\(timeUpSecond)")
                    .offset(x:5, y:5)
                ZStack{
                    Rectangle()
                        .frame(width: 100,height: 60)
                        .border(/*@START_MENU_TOKEN@*/Color.black/*@END_MENU_TOKEN@*/)
                        .opacity(0.3)
                    Text("Score:\(score)")
                        .offset(x:5, y:5)
                }
                Button(action: {
                    
                    withAnimation(.easeInOut(duration: 1)){
                        generateRandomFruits()
                        
                        //timeUpSecond=60
                    }
                }, label: {
                    Image(systemName: "shuffle")
                        .font(.largeTitle)
                })
                
                if showAlert==true{
                    Text("")
                        .alert("恭喜過關", isPresented: $showAlert, actions:{
                            
                            Button("下一關", action: {
                                //go to next 
                                conShowAlert = false
                                EmptyView()
                            })
                        },message:{
                            Text("")
                        })
                }
                else if(timeUP && !showAlert){
                    //var conshow=false
                    Text("")
                        .alert("要再玩一次嗎？", isPresented: $conShowAlert,actions:{
                            Button("再玩一次", action: {
                                generateRandomFruits()
                                //  stopTimer()
                                score = 0
                                combo = 0
                                noActTime = 0
                                startTimer()
                            })
                            Button("取消", action: {
                                //generateRandomFruits()
                                //  stopTimer()
                                score = 0
                                combo = 0
                                noActTime = 0
                                //startTimer()
                            })
                        },message:{
                            Text("")
                        })
                    
                } 
                
            }
        }
        VStack(spacing:0){
            ForEach(0..<gamerow){
                row in
                HStack(spacing:0){
                    ForEach(0..<gamecol){
                        col in 
                        ZStack{
                            
                            Rectangle()
                                .frame(width: 40,height: 40)
                                .border(Color.black)
                                .opacity(0.3)
                            if fruitBlocks[row][col].isHint{
                                Rectangle()
                                    .stroke(Color.white)
                                    .frame(width: 40,height: 40)
                                    .background(.white)
                                    .scaleEffect(1.2)
                                    .opacity(0.8)
                                    .blur(radius: 5)
                            }
                            //隨機在fruits內挑一個來呈現
                            if !(fruitBlocks[row][col].image=="") && (!timeUP){
                                Image(fruitBlocks[row][col].image)
                                    .resizable()
                                    .aspectRatio(contentMode: .fit)
                                    .frame(width:40, height:40)
                                    .offset(x: viewStateFruits[row][col].width,y: viewStateFruits[row][col].height)
                                    .gesture(DragGesture()
                                        .onChanged{
                                            value in
                                            
                                            if abs(value.translation.width)>abs(value.translation.height)
                                            {
                                                //horizontal
                                                
                                                if value.translation.width > 40{
                                                    
                                                    viewStateFruits[row][col].width = 40
                                                    
                                                }
                                                else if value.translation.width < -40{
                                                    viewStateFruits[row][col].width = -40
                                                }
                                                else {
                                                    viewStateFruits[row][col].width = value.translation.width
                                                }
                                                if value.translation.width > 0 && col < gamecol-1{
                                                    //   print("right")
                                                    fruitBlocks[row][col].direction = Direction.right
                                                    viewStateFruits[row][col+1].width = -viewStateFruits[row][col].width 
                                                    
                                                }
                                                else if value.translation.width < 0 && col > 0{
                                                    fruitBlocks[row][col].direction = Direction.left
                                                    viewStateFruits[row][col-1].width = -viewStateFruits[row][col].width 
                                                } 
                                                
                                            }
                                            else{
                                                //vertical
                                                if value.translation.height > 40 {
                                                    viewStateFruits[row][col].height = 40
                                                    
                                                }
                                                else if value.translation.height < -40 {
                                                    viewStateFruits[row][col].height = -40
                                                }
                                                else{
                                                    viewStateFruits[row][col].height = value.translation.height
                                                }
                                                if value.translation.height > 0 && row < gamerow - 1{
                                                    fruitBlocks[row][col].direction = Direction.down
                                                    viewStateFruits[row+1][col].height = -viewStateFruits[row][col].height 
                                                    
                                                }
                                                else if value.translation.height < 0 && row > 0{
                                                    fruitBlocks[row][col].direction = Direction.up
                                                    viewStateFruits[row-1][col].height = -viewStateFruits[row][col].height 
                                                } 
                                                
                                            }
                                            
                                            
                                        }
                                        .onEnded{
                                            (value) in
                                            if canSwipe(row:row,col:col){
                                                doSwipe(row: row, col: col)
                                            }
                                            else{
                                                withAnimation(.easeInOut(duration: 0.3)){
                                                    viewStateFruits[row][col] = .zero
                                                    if col < gamecol-1{
                                                        viewStateFruits[row][col+1] = .zero
                                                    }
                                                    if col > 0{
                                                        viewStateFruits[row][col-1] = .zero
                                                    }
                                                    if row > 0 {
                                                        viewStateFruits[row-1][col] = .zero
                                                    }
                                                    if row < gamerow-1{
                                                        viewStateFruits[row+1][col] = .zero
                                                    }
                                                }
                                            }
                                            
                                            
                                            
                                        }
                                    )
                                
                            }
                            
                        }
                    }
                    
                }
                
            }
        }
        .onAppear{
            trendNum[4] = trendNum[4]+1
          //  games[4].trendNum+=1
        }
        .onAppear{
            startTimer()
            generateRandomFruits()
        }
        .onDisappear(){
            
            stopTimer()
            
        }
    }
}
//struct GRID{
//    var row:Int
//    var col:Int
//    init(r:Int ,c:Int){
//        self.row = r
//        self.col = c
//    }
//}
//enum Direction{
//    case right,left,up,down,none
//}
//struct fruit: Identifiable,Equatable{
//    var id = UUID()
//    var image : String
//    var direction: Direction = Direction.none
//    var isHint: Bool = false
//    var OFFset: CGSize = CGSize.zero
//    var isUserInteractionEnabled = false
//}
var fruitsFree = [
    fruit(image: "IMG_0448"),
    fruit(image: "IMG_0449"),
    fruit(image: "IMG_0450"),
    fruit(image: "IMG_0451"),
    fruit(image: "IMG_0452"),
    fruit(image: "IMG_0453"),
    fruit(image: "IMG_0454"),
    fruit(image: "IMG_0455"),
    fruit(image: "IMG_0456"),
    fruit(image: "IMG_0457"),
    fruit(image: "IMG_0458"),
    fruit(image: "IMG_0459")
    
]









    

      
 ```

  <img src="https://raw.githubusercontent.com/AmilyC/SwiftUI_final_project/main/FreeGameFruit.png">
     </td>
  </tr>
</table>
