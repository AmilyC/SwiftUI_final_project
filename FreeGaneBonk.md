 <h1>ab</h1>
<h2>1A2B</h2>
<table>
  <tr>
    
    
      
 ```swift
  

import SwiftUI
import AVFoundation

struct FreeGameBonk: View {
    @State var standardEmoji = randomEmoji()
    @State var emojiToFind = randomEmoji()
    @State var randomEmojiArray = generateRandomEmojiArray()
    @State var score = -1
    @State var start = CFAbsoluteTimeGetCurrent()
    @State var end = CFAbsoluteTimeGetCurrent()
    @State var endTimer = false
    @State var showAlert = true
    @State var emoji: [EmojiRow] = generateEmoji(emojiToFind: "", standardEmoji: "")
     @AppStorage("trendNum") var trendNum = [0,0,0,0,0,0,0,0]
    @State private var nextGame = false
   // @AppStorage("trendNum") var trendNum :Int = 0
    var body: some View {
        VStack(spacing: 15) {
            Spacer()
            Header()
            Emojis(standardEmoji: $standardEmoji, emojiToFind: $emojiToFind, score: $score, start: $start, end: $end, endTimer: $endTimer, showAlert: $showAlert, emoji: $emoji)
            Buttons2(standardEmoji: $standardEmoji, emojiToFind: $emojiToFind, randomEmojiArray: $randomEmojiArray, score: $score, start: $start, end: $end, endTimer: $endTimer, showAlert: $showAlert, emoji: $emoji)
            Spacer()
        }
        .onAppear{
            trendNum[6] = trendNum[6]+1
           // games[6].$trendNum += 1
          //  games[6].trendNum+=1
        }
        .frame(minWidth: 0, maxWidth: .infinity, minHeight: 0, maxHeight: .infinity)
        .background{
            Image("b_back")
                .resizable()
                .scaledToFill()
            //                .opacity(0.3)
                .clipped()
            NavigationLink(
                destination: storyView(), isActive: $nextGame,
                label:{
                    EmptyView()
                }
            ).hidden()
        }
        
        
        //        .background(
        //            Background(randomEmojiArray: randomEmojiArray)
        //        )
        
    }
    
}

//struct Emoji {
//    let id = UUID()
//    let emoji: String
//}

//struct EmojiRow {
//    let id = UUID()
//    var emojis: [Emoji]
//}


//struct Header: View {
//    var body: some View {
//        
//        Image("eye")
//            .resizable()
//            .scaledToFit()
//    }
//}

//struct EmojiButtonContent: View {
//    var emojiStr: Emoji
//    var body: some View {
//        HStack(spacing: 12) {
//            Text(emojiStr.emoji).font(.title3)
//        }.padding([.leading, .trailing], 15).padding([.top, .bottom], 15).background(
//            Image("standardeye")
//                .resizable()
//                .scaledToFit()
//                .foregroundColor(.red)
//                .opacity(0.8))
//    }
//}



//struct EmojiButton: View {
//    var emojiStr: Emoji
//    @Binding var standardEmoji: String
//    @Binding var emojiToFind: String
//    @Binding var end: Double
//    @Binding var score: Int
//    @Binding var endTimer: Bool
//    @Binding var showAlert: Bool
//    @Binding var emoji: [EmojiRow]
//    var body: some View {
//        Button(action: {
//            if(emojiStr.emoji == emojiToFind){
//                end = CFAbsoluteTimeGetCurrent()
//                emoji = generateEmoji(emojiToFind: emojiToFind, standardEmoji: standardEmoji)
//                score += 1
//                if(score == 20){
//                    emoji = generateEmoji(emojiToFind: "", standardEmoji: "")
//                    end = CFAbsoluteTimeGetCurrent()
//                    endTimer = true
//                    showAlert = true
//                }
//            }
//        }){
//            EmojiButtonContent(emojiStr: emojiStr)
//        }.disabled(!(emojiStr.emoji == emojiToFind))
//    }
//}

//struct Emojis: View {
//    @Binding var standardEmoji: String
//    @Binding var emojiToFind: String
//    @Binding var score: Int
//    @Binding var start: Double
//    @Binding var end: Double
//    @Binding var endTimer: Bool
//    @Binding var showAlert: Bool
//    @Binding var emoji: [EmojiRow]
//    var body: some View {
//        VStack(spacing: 12){
//            ForEach(emoji, id: \.id){
//                emojiRow in HStack{
//                    ForEach(emojiRow.emojis, id: \.id){
//                        emojiStr in EmojiButton(emojiStr: emojiStr, standardEmoji: $standardEmoji, emojiToFind: $emojiToFind, end: $end, score: $score, endTimer: $endTimer, showAlert: $showAlert, emoji: $emoji)
//                    }
//                }
//            }
//        }
//    }
//}

struct StartButton2: View {
    @Binding var standardEmoji: String
    @Binding var emojiToFind: String
    @Binding var randomEmojiArray: [String]
    @Binding var score: Int
    @Binding var start: Double
    @Binding var end: Double
    @Binding var endTimer: Bool
    @Binding var showAlert: Bool
    @Binding var emoji: [EmojiRow]
    
    @State private var nextGame = false
    
    var body: some View {
        Button(action: {
            standardEmoji = randomEmoji()
            emojiToFind = randomEmoji()
            emoji = generateEmoji(emojiToFind: emojiToFind, standardEmoji: standardEmoji)
            score = 0
            start = CFAbsoluteTimeGetCurrent()
            end = start
            Timer.scheduledTimer(withTimeInterval: 1, repeats: true){ 
                timer in
                if endTimer {
                    timer.invalidate()
                    endTimer = false
                }
                else {
                    randomEmojiArray = generateRandomEmojiArray()
                    end = CFAbsoluteTimeGetCurrent()
                }
            }
        })
        {
            HStack(spacing: 15) {
                if score >= 0 {
                    
                    Text("RESTART").font(.title3).fontWeight(.bold).foregroundColor(Color.red)
                        .alert(isPresented: $showAlert) {
                            Alert(title: Text("時間到！"), message: Text("You spend \(Int(round(end - start)))"+"s"), dismissButton: .default(Text("OK")))
                        }
                    
                }
                else{
                    Text("START").font(.title3).fontWeight(.bold).foregroundColor(Color.red)
                }
            }.padding([.leading, .trailing], 15).padding([.top, .bottom], 12).background(Color.clear).overlay(RoundedRectangle(cornerRadius: 10).stroke(lineWidth: 2).foregroundColor(Color.red))}
        NavigationLink(
            destination: freeGameView(), isActive: $nextGame,
            label:{
                EmptyView()
            }
        ).hidden()
        
    }
}

//struct ScoreButton: View {
//    @Binding var score: Int
//    var body: some View {
//        HStack(spacing: 12) {
//            Text(String(score)+"/20").font(.title3).fontWeight(.bold).foregroundColor(Color.red)
//        }.padding([.leading, .trailing], 15).padding([.top, .bottom], 12).background(Color.clear).overlay(RoundedRectangle(cornerRadius: 10).stroke(lineWidth: 2).foregroundColor(Color.red))
//    }
//}

//struct TimeButton: View {
//    @Binding var start: Double
//    @Binding var end: Double
//    var body: some View {
//        HStack(spacing: 12) {
//            Text(
//                String(
//                    Int(round(
//                        end - start
//                    ))
//                )+"s").font(.title3).fontWeight(.bold).foregroundColor(Color.red)
//        }.padding([.leading, .trailing], 15).padding([.top, .bottom], 12).background(Color.clear).overlay(RoundedRectangle(cornerRadius: 10).stroke(lineWidth: 2).foregroundColor(Color.red))
//    }
//}

struct Buttons2: View {
    @Binding var standardEmoji: String
    @Binding var emojiToFind: String
    @Binding var randomEmojiArray: [String]
    @Binding var score: Int
    @Binding var start: Double
    @Binding var end: Double
    @Binding var endTimer: Bool
    @Binding var showAlert: Bool
    @Binding var emoji: [EmojiRow]
    var body: some View {
        HStack {
            if score == -1 || score >= 20 {
                StartButton2(standardEmoji: $standardEmoji, emojiToFind: $emojiToFind,  randomEmojiArray: $randomEmojiArray, score: $score, start: $start, end: $end, endTimer: $endTimer, showAlert: $showAlert, emoji: $emoji)
            }
            else{
                ScoreButton(score: $score)
            }
            if score != -1 {
                TimeButton(start: $start, end: $end)
            }
        }
    }
}

//struct Background: View {
//    var randomEmojiArray: [String]
//    var body: some View {
//        Image("itachi")
//            .resizable()
//            .scaledToFill()
//            .opacity(0.2)
//    }
//}

//func generateEmoji(emojiToFind: String, standardEmoji: String) -> [EmojiRow] {
//    var emoji: [EmojiRow] = []
//    let randomNumber = Int.random(in: 0..<20)
//    for x in 0...5 {
//        var newRow = EmojiRow(emojis: [])
//        for y in 0...4 {
//            if((x*5 + y) == randomNumber){
//                newRow.emojis.append(Emoji(emoji: emojiToFind))
//            }
//            else{
//                newRow.emojis.append(Emoji(emoji: standardEmoji))
//            }
//        }
//        emoji.append(newRow)
//    }
//    return emoji
//}

//func randomEmoji() -> String {
//    return String(UnicodeScalar(Array(0x1F300...0x1F3F0).randomElement()!)!)
//}

//func generateRandomEmojiArray() -> [String] {
//    var array: [String] = []
//    for _ in 0...750 {
//        array.append(randomEmoji())
//    }
//    return array
//}




    

      
 ```

  <img src="https://raw.githubusercontent.com/AmilyC/Yzu-swiftui/main/Hw1.png">
     </td>
  </tr>
</table>
