<h1>Welcome</h1>
<h2>初始畫面</h2>
<table>
  <tr>
   
    
      
 ```swift
  

import SwiftUI

struct WelcomeView: View {
    
    @State var showDetailView = false
    @State var selectCourse:GameInfo?//value may be empty
    //@AppStorage("trendNum") var trendNum:Int = 0
    @State var upDate: Bool = true
   // @State var sortedGames:[GameInfo] = games.sorted(by: { $0.trendNum >= $1.trendNum})
    
    
    var body: some View {
        //
        // withIndex = courses.enumerated().map({$0})
        VStack{
            Text("")
            TitleView(text: "遊戲說明")
                .foregroundStyle(.primary)
            Text("歡迎來到\n斯威夫特的妙妙屋！\n在這裡有眾多遊戲任君挑選\n除了單一遊戲之外，\n我們也將遊戲組合成一個闖關遊戲\n等你來挑戰！\n一起來體驗遊戲的樂趣吧！")
                .padding()
                //.foregroundStyle(.secondary)
        }
       
        .frame(minWidth: 0, maxWidth: .infinity, minHeight: 0, maxHeight: .infinity)
         .background(Color(red:168/255,green:216/255,blue:130/255))
        
        
    }
    
}
struct BasicImageRowForTrend: View{
    var thisCourse:GameInfo
    @State var number:Int = 0
    var body: some View{
        HStack{
            Text(String(number))
                .font(.headline)
            
            //foregroundStyle(.secondary)
                .foregroundStyle(.black)
            
            
            Image(thisCourse.image)
                .resizable()
                .frame(width: 40 ,height: 40)
                .cornerRadius(5)
            Text(thisCourse.courseNameTW)
                .font(.headline)
                .foregroundStyle(.secondary)
                .foregroundStyle(.white)
            
            Spacer()
            
            Image(systemName: "ellipsis.circle.fill")
                .foregroundStyle(.secondary)
                .foregroundStyle(Color.white)
        }
        
        
    }
}



struct TitleView: View{
    @AppStorage("UserName") var UserName:String = ""
    var text:String
    var body: some View{
        Text("")
        HStack(){
            
            Text(text)
                .font(.largeTitle)
                .fontWeight(.heavy)
                .foregroundStyle(.primary)
                .foregroundColor(Color.black)
            //.listRowBackground(tabColor()[1])
            //.background(tabColor()[1])
                .frame(minWidth: 0, idealWidth: 50,maxWidth: .infinity,minHeight: 0,idealHeight: 200, alignment: .center)
            
                .fontWeight(.heavy)
            
        }
        
        
    }
}

      
 ```
  <img src="IMG_0551.png">
     </td>
 
  </tr>
</table>
