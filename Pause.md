<h1>Pause</h1>
<h2>音樂暫停</h2>
<table>
  <tr>
   
    
      
 ```swift
  


import SwiftUI
import AVFoundation

struct Pause:View{
//    @State var player = AVPlayer()
    @Environment(\.presentationMode) var presentationMode: Binding<PresentationMode>
    @AppStorage("vol") var ti: Int = 0
    @AppStorage("play") var play: Bool = true
//    @State private var play = true
    @State private var vol = 0
    @AppStorage("UserC") var UserC:Int = 0
    
    var body:some View{
        VStack{
            Text("")
//                .onAppear {
//                    let url = Bundle.main.url(forResource: "Christmas", withExtension: "mp3")!
//                    let playerItem = AVPlayerItem(url: url)
//                    player.replaceCurrentItem(with: playerItem)
//                    player.play()
//                    //                    player.volume = 2
//                }
            
            HStack{
                Button(action: {
//                    player.pause()
                play = false
                    
                }, label: {
                    Text("PAUSE")
                        .padding(.all,10)
                        .frame(width: 180, height: 80, alignment: /*@START_MENU_TOKEN@*/.center/*@END_MENU_TOKEN@*/)
                        .font(.system(size: 45, design: .serif))
                        .background(Color(red: 25/255, green: 74/255, blue: 0/255))
                        .foregroundColor(.white)
                        .cornerRadius(20)
                })
                Button(action: {
//                    player.play()
                    play = true
                    
                }, label: {
                    Text("start")
                        .padding(.all,10)
                        .frame(width: 180, height: 80, alignment: /*@START_MENU_TOKEN@*/.center/*@END_MENU_TOKEN@*/)
                        .font(.system(size: 45, design: .serif))
                        .background(Color(red: 25/255, green: 74/255, blue: 0/255))
                        .foregroundColor(.white)
                        .cornerRadius(20)
                })
                Button(action: {
//                    player.volume = player.volume + 0.5;
                    vol += 1;
//                    $UserC = 1;
                    ti = vol;
                }, label: {
                    Text("+")
                        .padding(.all,10)
                        .frame(width: 180, height: 80, alignment: /*@START_MENU_TOKEN@*/.center/*@END_MENU_TOKEN@*/)
                        .font(.system(size: 45, design: .serif))
                        .background(Color(red: 25/255, green: 74/255, blue: 0/255))
                        .foregroundColor(.white)
                        .cornerRadius(20)
                })
                Button(action: {
//                    player.volume = player.volume - 0.5;
                    
                }, label: {
                    Text("-")
                        .padding(.all,10)
                        .frame(width: 180, height: 80, alignment: /*@START_MENU_TOKEN@*/.center/*@END_MENU_TOKEN@*/)
                        .font(.system(size: 45, design: .serif))
                        .background(Color(red: 25/255, green: 74/255, blue: 0/255))
                        .foregroundColor(.white)
                        .cornerRadius(20)
                })
            }
            
        }
    }
}

}
      
 ```
  <img src="https://raw.githubusercontent.com/AmilyC/Yzu-swiftui/main/Hw1.png">
     </td>
 
  </tr>
</table>
