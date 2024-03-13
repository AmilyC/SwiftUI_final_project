<h1>setting</h1>
<h2>設定頁面</h2>
<table>
  <tr>
   
    
      
 ```swift
import SwiftUI
import AVFoundation

//var isPlay = true

struct SettingView: View {
    @State var player = AVPlayer()
    //    @State var IsDeepScheme = false;
//    @AppStorage("isPlay") var isPlay :Bool = false
    let displayFontType = [".default",".rounded",".monospaced",".serif"]
    @State var displayFontSelected = 0
    @State var IsDeepScheme = true;
    @State var colorArray:Array = [255.0,255.0,255.0]
    @State var stepperValue = 0
    @State var sliderValue = 0.0
    @State var date = Date()
    @AppStorage("UserName") var UserName:String = ""
    @AppStorage("trendNum") var trendNum = [0,0,0,0,0,0,0,0]
    
    @AppStorage("vol") var vol: Double = 0.50
    @AppStorage("play") var play: Bool = false
    
    
    //.standard.array(forKey: "trendN")
    var body: some View {
        VStack{
            Text("")
//                .onAppear {
//                    let url = Bundle.main.url(forResource: "Christmas", withExtension: "mp3")!
//                    let playerItem = AVPlayerItem(url: url)
//                    player.replaceCurrentItem(with: playerItem)
//                    //                    player.play()
//                    if(play == true){
//                        player.play()
//                    }
//                    else if(play == false){
//                        player.pause()
//                    }
//                }
            NavigationView{
                Form(
                    content: {
                        Section(content: {
                            TextField("請輸入您的名字", text: $UserName)
                        }, header: {
                            Text("使用者名稱")
                        })
//                        Section(header: Text("字型設定"), content: {
//                            Picker(selection: $displayFontSelected, label: Text("字型選擇")){
//                                ForEach(0..<displayFontType.count, id: \.self, content:{
//                                    Text(self.displayFontType[$0])  
//                                })
//                            }
//                        })
                        Section(header: Text("播放音樂"), content: {
                            Toggle(isOn: $play){
                                Text("播放\(String(play))")
                            }
                                
                        })
                        
//                        Section(header: Text("計數器"), content: {
//                            Stepper( onIncrement: {stepperValue+=1;}, onDecrement: {stepperValue-=1},
//                                     label: {
//                                Text("Stepper \(stepperValue)")
//                            })
//                        })
                        Section(header: Text("音量大小\(vol,specifier: "%.2f")"), content: {
                            Slider(value: $vol, in: 0...1, step: 0.01)
                        })
//                        Section(header: Text("日期"), content: {
//                            DatePicker("\(date.formatted(date: .numeric, time: .omitted))", selection: $date, displayedComponents: [.date])
//                        })
                    }
                )
                .navigationTitle("Settings 設定")
                //                .onChange(of: IsDeepScheme){
                //                    old, new in
                //                    if new{
                //                        player.play()
                //                    }
                //                    else{
                //                        player.pause()
                //                    }
                //                }
            }
            .navigationViewStyle(StackNavigationViewStyle())
        }
        
    }
    //    func onChange<V>(
    //        of value: V,
    //        initial: Bool = false,
    //        _ action: @escaping (V, V) -> Void
    //        
    //    )
}
extension Array: RawRepresentable where Element: Codable{
    public init?(rawValue: String){
        guard let data = rawValue.data(using: .utf8),
              let result = try?JSONDecoder().decode([Element].self, from: data)
                else{ return nil }
        self = result
    }
    public var rawValue: String{
        guard let data = try?JSONEncoder().encode(self),
              let result = String(data: data, encoding: .utf8)
                else{
            return "[]"
        }
        return result
    }
}

      
 ```
  <img src="https://raw.githubusercontent.com/AmilyC/Yzu-swiftui/main/Hw1.png">
     </td>
 
  </tr>
</table>
