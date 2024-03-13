<h1>snakeview</h1>
<h2>貪吃蛇 自由模式</h2>
<table>
  <tr>
   
    
      
 ```swift
  import SwiftUI
import Foundation

struct inSnake: View {
    @State var isRain = true
    @State private var nextGame = false
    var body: some View {
        
        TabView {
            snakeStoryView()
                .tabItem{
                    //HStack{
                    //Image(systemName: "cube.box")
                    //Text("1A2B")
                    //}
                }
        }.background(
            NavigationLink(
                destination: inCrash(), isActive: $nextGame,
                label:{
                    EmptyView()
                }
            ).hidden()
        )
    }
}

enum Directions1{
    case up,down,left,right,null
}
let SnakeDirec:Array=[
    "SnakeHeadD","SnakeHeadL","SnakeHeadR","SnakeHeadU"
]
let imageNames:Array=[
    "Cave","Jungle","Ocean","Arctic","Desert","Heaven","Hell"
]
struct snakeStoryView: View {
    @State var MapCount:Int = 0
    @State var currentImage:Int = 0 
    @State private var StartPosition: CGPoint = .zero
    @State private var positions = [CGPoint(x: 0,y: 0)]
    @State private var FoodPosition = CGPoint(x: 0,y: 0)
    @State private var isStarted = true
    @State var GameOver = false
    @State private var score = 0
    @State private var direction = Directions1.null
    @State private var nextGame = false
    let SnakeSize:CGFloat=50
    @State var alertTypes:Game? = nil
    enum Game{
        case fail
        case pass
    }
    let timer = Timer.publish(every: 0.12, on: .main, in: .common).autoconnect()
    var body: some View{
        ZStack{            Image(imageNames[MapCount]).resizable().ignoresSafeArea()
            
            ZStack{
                VStack{
                    HStack{
                        HStack{
                            Text("分數： ").font(.headline)
                            Text("\(score)")
                                .font(.title2)
                                .fontWeight(.medium)
                        }
                        Spacer()
                        Button(action:{
                            Start()
                        },label:{
                            Text("重新開始")
                                .font(.footnote)
                                .fontWeight(.medium)
                                .foregroundColor(.black)
                                .frame(width: 65,height:30)
                                .background(Color.secondary)
                                .clipShape(RoundedRectangle(cornerRadius: 25))
                        })
                    }.padding()
                        .foregroundColor(.white)
                    Spacer()
                }
                Image(SnakeDirec[currentImage])
                    .resizable()
                    .frame(width:SnakeSize*1.25,height: SnakeSize*1.25)
                    .position(positions[0])
                ForEach(1..<positions.count,id:\.self){
                    index in
                    Circle()
                        .fill(Color(red:152/255,green:194/255,blue:86/255))
                        .overlay(Capsule().stroke(Color.black,lineWidth: 2))
                        .frame(width:SnakeSize*1,height: SnakeSize*1)
                        .position(positions[index])
                }
                Image("Fruit")
                    .resizable()
                    .frame(width:SnakeSize*1.2,height:SnakeSize*1.2)
                    .position(FoodPosition)
                
            }.onAppear{
                withAnimation(.easeInOut){
                    FoodPosition=changePosition()
                    positions[0]=changePosition()
                    
                }
            }
            NavigationLink(
                destination: inCrash(), isActive: $nextGame,
                label:{
                    EmptyView()
                }
            ).hidden()
        }
        .alert(isPresented:$GameOver)
        {
            Result()
        }
        .gesture(
            DragGesture()
                .onChanged{gesture in
                    if isStarted{
                        withAnimation{
                            StartPosition=gesture.location
                            isStarted.toggle()
                        }
                    }
                }
                .onEnded{ gesture in
                    let DistanceX=abs(gesture.location.x-StartPosition.x)
                    let DistanceY=abs(gesture.location.y-StartPosition.y)
                    
                    if StartPosition.y<gesture.location.y && DistanceY>DistanceX && direction != Directions1.up
                    {
                        direction=Directions1.down
                        currentImage=0
                    }
                    else if StartPosition.y>gesture.location.y && DistanceY>DistanceX && direction != Directions1.down
                    {
                        direction=Directions1.up
                        currentImage=3
                    }
                    else if StartPosition.x>gesture.location.x && DistanceY<DistanceX && direction != Directions1.left
                    {
                        direction=Directions1.right
                        currentImage=1
                    }
                    else if StartPosition.x<gesture.location.x && DistanceY<DistanceX && direction != Directions1.right
                    {
                        direction=Directions1.left
                        currentImage=2
                    }
                    isStarted.toggle()
                }
        )
        
        .onReceive(timer)
        {
            time in if !GameOver{
                withAnimation(.linear(duration:0.16))
                {
                    
                    changeDirection()
                }
                if positions[0]==FoodPosition
                //if(abs(positions[0].x-FoodPosition.x))*(abs(positions[0].y-FoodPosition.y))<=2
                {
                    withAnimation(.smooth()){
                        positions.append(positions[0])
                    }
                    FoodPosition=changePosition()
                    score += 1
                    if (score%4==0)
                    {
                        withAnimation(.easeInOut(duration:1.5 )){
                            MapCount+=1
                        }
                    }
                    if (score==2)
                    {
                        GameOver.toggle()
                        alertTypes = .pass
                        self.timer.upstream.connect().cancel()
                    }
                }
            }
        }
        .background{
            Image("bombBack")
                .resizable()
                .scaledToFill()
                .opacity(0.3)
                .clipped()
        }
    }
    let minX=UIScreen.main.bounds.minX
    let maxX=UIScreen.main.bounds.maxX
    let minY=UIScreen.main.bounds.minY
    let maxY=UIScreen.main.bounds.maxY
    
    func changePosition()->CGPoint{
        let row = Int(maxX/SnakeSize)
        let col = Int(maxY/SnakeSize)
        let randomX = Int.random(in:1..<row)*Int(SnakeSize)
        let randomY = Int.random(in:1..<col-1)*Int(SnakeSize)
        //let randomPosittion = CGPoint(x:1150,y:randomY)
        let randomPosittion = CGPoint(x:randomX,y:randomY)
        return randomPosittion
    }
    
    /* func restartTimer(){
     let buff = 0.2 - Double(score/100)
     let TimerBuff = TimeInterval(buff) 
     let paratimer = Timer.publish(every: TimerBuff, on: .main, in: .common)
     } */
    
    func changeDirection(){
        if positions[0].x < minX || positions[0].x > maxX && !GameOver{
            GameOver.toggle()
            withAnimation(.easeInOut(duration:0.5 )){
                MapCount = 6
                alertTypes = .fail
            }
        }
        else if positions[0].y < minY || positions[0].y > maxY && !GameOver{
            GameOver.toggle()
            withAnimation(.easeInOut(duration:0.5 )){
                MapCount = 6
                alertTypes = .fail
            }
        }
        
        var prev = positions[0]
        let speedbonus = CGFloat(0)
        if direction == .null{
            positions[0].x += 0  
        }
        else if direction == .down{
            positions[0].y += (SnakeSize + speedbonus)
        }
        else if direction == .up{
            positions[0].y -= (SnakeSize + speedbonus)
        }
        else if direction == .left{
            positions[0].x += (SnakeSize + speedbonus)
        }else {
            positions[0].x -= (SnakeSize + speedbonus)
        }
        
        for index in 1..<positions.count{
            
            let current = positions[index]
            positions[index]=prev
            prev = current
            
        }
        if (positions.count > 5)
        {
            for index in 3..<positions.count{
                if ( positions[index] == positions[0])
                {
                    GameOver.toggle()
                    withAnimation(.easeInOut(duration:0.5 )){
                        MapCount = 6
                        alertTypes = .fail
                    }
                }
            }
        }
        
    }
    
    func Start()
    {
        withAnimation(.easeInOut){
            MapCount=0
            score = 0
            positions=[CGPoint(x: 0, y:0)]
            GameOver=false
            alertTypes = nil
            isStarted=true
            direction = .null
            FoodPosition=changePosition()
            positions[0]=changePosition()
            changeDirection()
            nextGame = false
        }
    }
    func Result()->Alert{
        switch alertTypes {
        case .fail:
            return Alert(title:Text("遊戲結束"),message:Text("你的得分 \(score)"),primaryButton:.default(Text("回到標題"),action:{
                GameOver.toggle()
                isStarted.toggle()
            }),secondaryButton: .default(Text("重新開始"),action:{
                Start()
            }))
        case .pass:
            return Alert(title: Text("恭喜通關🎉"),message: Text("隨後，玩家來到了寶石仙境，這是一個充滿寶石的奇幻世界。你需要在60秒內收集到50分，才能獲得通往下一階段。繼續展開旅程"), dismissButton:.default(Text( "前往下一個關卡"),action:{
                // NextView()
                direction = .null
                nextGame = true
            })
            )
        default: return Alert(title:Text("error"))
        }
        
    }
}

 ```
  <img src="https://raw.githubusercontent.com/AmilyC/Yzu-swiftui/main/Hw1.png">
     </td>
 
  </tr>
</table>
