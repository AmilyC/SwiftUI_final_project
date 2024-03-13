<h1>ab</h1>
<h2>1A2B</h2>
<table>
  <tr>
    
    
      
 ```swift
  
import SwiftUI
import Foundation
struct ooxxView: View {
     @AppStorage("trendNum") var trendNum = [0,0,0,0,0,0,0,0]
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
            TabView{
                //Group{
                SinglePlayer()
                    .tabItem {     
                        Image(systemName: "rosette")
                        Text("單人模式")
                    }
                Doubleplayer()
                    .tabItem {  
                    Image(systemName: "list.dash")
                    Text("雙人模式")
                }
            }
            
        }
        .onAppear{
            trendNum[0]=trendNum[0]+1
         //   games[0].trendNum+=1
        }
        

    }
}

//import SwiftUI

struct Doubleplayer: View {
    @State private var moves = ["","","","","","","","",""]
    @State private var endGameText = ""
    @State private var gameEnded = false
    @State private var turns = 1
    
    private var ranges = [(0..<3),(3..<6),(6..<9)]
    
    var body: some View {
        VStack {
            Text("雙人模式")
                .alert(endGameText, isPresented: $gameEnded) {
                    Button("Reset", role: .destructive)
                    {
                        endGameText = "雙人模式"
                        moves = ["","","","","","","","",""]
                    }
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
                                        playerTap(index: i,index2: turns)
                                        turns+=1
                                    }
                            )
                    }
                }
            }
            Spacer()
            Button("Reset") {
                endGameText = "雙人模式"
                moves = ["","","","","","","","",""]
            }
            .padding(.bottom,70)
        }
    }
    
    func playerTap(index: Int , index2: Int) {
        if moves[index] == "" {
            if index2%2 == 0
            {
                moves[index] = "X"
            }else{
                moves[index] = "O"
            }
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
}


//struct Doubleplayer: View {
//    @State private var moves = ["","","","","","","","",""]
//    @State private var endGameText = ""
//    @State private var gameEnded = false
//    @State private var turns = 1
//    
//    private var ranges = [(0..<3),(3..<6),(6..<9)]
//    
//    var body: some View {
//        VStack {
//            Text("雙人模式")
//                .alert(endGameText, isPresented: $gameEnded) {
//                    Button("Reset", role: .destructive)
//                    {
//                        endGameText = "雙人模式"
//                        moves = ["","","","","","","","",""]
//                    }
//                }
//                .padding(.top,50)
//                .font(/*@START_MENU_TOKEN@*/.title/*@END_MENU_TOKEN@*/)
//            Spacer()
//            ForEach(ranges, id: \.self) { range in
//                HStack {
//                    ForEach(range, id: \.self) { i in
//                        XOButton(letter: $moves[i])
//                            .simultaneousGesture(
//                                TapGesture()
//                                    .onEnded { _ in
//                                        playerTap(index: i,index2: turns)
//                                        turns+=1
//                                    }
//                            )
//                    }
//                }
//            }
//            Spacer()
//            Button("Reset") {
//                endGameText = "雙人模式"
//                moves = ["","","","","","","","",""]
//            }
//            .padding(.bottom,70)
//        }
//    }
//    
//    func playerTap(index: Int , index2: Int) {
//        if moves[index] == "" {
//            if index2%2 == 0
//            {
//                moves[index] = "X"
//            }else{
//                moves[index] = "O"
//            }
//        }
//        
//        for letter in ["X", "O"] {
//            if check(list: moves, letter: letter) {
//                endGameText = "\(letter) 贏了!"
//                gameEnded = true
//                break
//            }else if checkeq(list: moves){
//                endGameText = "平手"
//                gameEnded = true   
//                break
//            }
//        }  
//    }
//}

//struct SinglePlayer: View {
//    @State private var moves = ["","","","","","","","",""]
//    @State private var endGameText = "單人模式"
//    @State private var gameEnded = false
//    private var ranges = [(0..<3),(3..<6),(6..<9)]
//    
//    
//    var body: some View {
//        VStack {
//            Text(endGameText)
//                .alert(endGameText, isPresented: $gameEnded) {
//                    Button("重來", role: .destructive)
//                    {
//                        endGameText = "單人模式"
//                        moves = ["","","","","","","","",""]
//                    }
//                }
//                .padding(.top,50)
//                .font(/*@START_MENU_TOKEN@*/.title/*@END_MENU_TOKEN@*/)
//            Spacer()
//            ForEach(ranges, id: \.self) { range in
//                HStack {
//                    ForEach(range, id: \.self) { i in
//                        XOButton(letter: $moves[i])
//                            .simultaneousGesture(
//                                TapGesture()
//                                    .onEnded { _ in
//                                        Tap(index: i)
//                                    }
//                            )
//                    }
//                }
//            }
//            Spacer()
//            Button("Reset") {
//                endGameText = "單人模式"
//                moves = ["","","","","","","","",""]
//            }
//            .padding(.bottom,70)
//        }
//    }
//    
//    func Tap(index: Int) {
//        if moves[index] == "" {
//            moves[index] = "X"
//            botMove()
//        }
//        
//        for letter in ["X", "O"] {
//            if check(list: moves, letter: letter) {
//                endGameText = "\(letter) 贏了!"
//                gameEnded = true
//                break
//            }else if checkeq(list: moves){
//                endGameText = "平手"
//                gameEnded = true     
//                break
//            }
//        }  
//    }
//    
//    func botMove() {
//        var availableMoves: [Int] = []
//        var movesLeft = 0
//        
//        for move in moves {
//            if move == "" {
//                availableMoves.append(movesLeft)
//            }
//            movesLeft += 1
//        }
//        
//        if availableMoves.count != 0 {
//            moves[availableMoves.randomElement()!] = "O"
//        }
//    }
//}
//import SwiftUI

struct SinglePlayer: View {
    @State private var moves = ["","","","","","","","",""]
    @State private var endGameText = "單人模式"
    @State private var gameEnded = false
    private var ranges = [(0..<3),(3..<6),(6..<9)]
    
    
    var body: some View {
        VStack {
            Text(endGameText)
                .alert(endGameText, isPresented: $gameEnded) {
                    Button("Reset", role: .destructive)
                    {
                        endGameText = "單人模式"
                        moves = ["","","","","","","","",""]
                    }
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
            Button("Reset") {
                endGameText = "單人模式"
                moves = ["","","","","","","","",""]
            }
            .padding(.bottom,70)
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
    func checkeq(list: [String]) -> Bool{
        var eq = true
        for i in 0 ... 8{
            if list[i] == ""{
                eq = false
                //print(list)
            }
        }
        return eq
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
        var winningpoint: [Int] = []
        if availableMoves.count != 0 {
            if availableMoves.count < 7{
                for move in availableMoves {
                    var temp: [String] = []
                    for lett in moves{
                        temp.append(lett)
                    }
                    temp[move] = "O"
                    if check(list: temp, letter: "O"){
                        winningpoint.append(move)
                    }
                }
                //print(winningpoint)
                //print(availableMoves)
                if !winningpoint.isEmpty{
                    moves[winningpoint.randomElement()!] = "O"
                }else{
                    for move in availableMoves {
                        var temp: [String] = []
                        for lett in moves{
                            temp.append(lett)
                        }
                        temp[move] = "X"
                        if check(list: temp, letter: "X"){
                            winningpoint.append(move)
                        }
                    }
                    if !winningpoint.isEmpty{
                        moves[winningpoint.randomElement()!] = "O"
                    }else{
                        moves[availableMoves.randomElement()!] = "O"
                    }
                }
            }else{
                if moves[4] != "X"{
                    moves[4] = "O"
                }else{
                    moves[availableMoves.randomElement()!] = "O"
                }
            }
        }
    }
}
func checkeq(list: [String]) -> Bool{
       var eq = true
    for i in 0 ... 8{
        if list[i] == ""{
            eq = false
            //print(list)
        }
    }
    return eq
}
func check(list: [String], letter: String) -> Bool {
        let winningSequences = [
        [ 0, 1, 2 ], [ 3, 4, 5 ], [ 6, 7, 8 ],
        [ 0, 4, 8 ], [ 2, 4, 6 ],
        [ 0, 3, 6 ], [ 1, 4, 7 ], [ 2, 5, 8 ],
    ]
    for sequence in winningSequences {
        var score = 0
        
        for match in sequence {
            if list[match] == letter {
                score += 1
                
                if score == 3 {
                    return true
                }
            }
        }
    }
    return false
}

struct XOButton: View {
    @Binding var letter: String
    @State private var degrees = 0.0
    
    var body: some View {
        ZStack {
            Circle()
                .frame(width: 120, height: 120)
                .foregroundColor(Color(red: 255/255, green: 224/255, blue: 184/255))
            Circle()
                .frame(width: 100, height: 100)
                .foregroundColor(Color(red:142/255,green:77/255,blue:0/255))
            Text(letter)
                .font(.system(size: 50))
                .bold()
        }
        .rotation3DEffect(.degrees(degrees), axis: (x: 0, y: 1, z: 0))
        .simultaneousGesture(
            TapGesture()
                .onEnded { _ in
                    withAnimation(.easeIn(duration: 0.1)) {
                        self.degrees -= 180
                    }
                }
        )
    }
}




    

      
 ```

  <img src="https://raw.githubusercontent.com/AmilyC/Yzu-swiftui/main/Hw1.png">
     </td>
  </tr>
</table>
