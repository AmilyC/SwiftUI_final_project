<h1>ContentView</h1>
<h2>控制遊戲主畫面</h2>
<table>
  <tr>
        
 ```swift
import SwiftUI
import AVFoundation

//var play = true

struct ContentView:View{
       @State var player = AVPlayer()
    
    @AppStorage("vol") var vol: Double = 0.5
    @AppStorage("play") var play: Bool = false
//    @AppStorage("isPlay") var isPlay :Bool = false
//    @State private var play = true
    @AppStorage("UserName") var UserName:String = ""
    var body:some View{
            
        VStack{
            HStack(){
                Text(UserName.isEmpty ? "" : UserName)
                    .font(.system(size: 30))
                    .foregroundColor(.white)
                    .background(.brown)
                    .clipShape(/*@START_MENU_TOKEN@*/Circle()/*@END_MENU_TOKEN@*/)
                    
                Text("Wellcome!")
                    .font(.largeTitle)
                    .fontWeight(.heavy)
                    .foregroundStyle(Color(red:220/255,green:92/255,blue:47/255))
                //.padding(.leading,0)
                    .fontWeight(.heavy)
            }
            .padding(.top,10)
            Text("")
                .onAppear {
//                    print(isPlay)
                    let url = Bundle.main.url(forResource: "Christmas", withExtension: "mp3")!
                    let playerItem = AVPlayerItem(url: url)
                    player.replaceCurrentItem(with: playerItem)
//                    player.play()
                    player.volume = 0.5
                    if(play == true){
//                        print("play")
                        player.play()
                    }
                    else{
                        player.pause()
                    }
                    //player.volume = 2
                }
            
            TabView{
                Group{
                    WelcomeView()
                        .tabItem {
                            Image(systemName: "house")
                            Text("Home")
                        }
                    freeGameView()
                        .tabItem {     
                            Image(systemName: "figure.yoga")
                            Text("自由模式")
                        }
                    storyView()
                        .tabItem {  
                            Image(systemName: "book")
                            Text("故事模式")
                        }
                    TrendView()
                        .tabItem {
                            Image(systemName: "flame.fill")
                            Text("Trends")
                        }
                    SettingView()
                        .tabItem {  
                            Image(systemName: "gearshape.fill")
                            Text("Setting")
                        }
                }
                .toolbarBackground(.brown, for: .tabBar)
               .toolbarColorScheme(.light,for: .tabBar)
                .toolbarBackground(.visible, for: .tabBar)
            }
            .tint(.black)
        }
        .onChange(of: play, perform: { value in
            if value{
                player.play()
            }
            else{
                player.pause()
            }
        })
        .onChange(of: vol, perform: { value in
            player.volume = Float(vol)
        })
    }
}      
 ```
  </tr>
</table>
