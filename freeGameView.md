 <h1>freeGameView</h1>
<h2>自由模式的介面</h2>
<table>
  <tr>
    
    
      
 ```swift
  

import SwiftUI

struct GameInfo: Identifiable {
    var image: String
    var courseNameUS: String
    var courseNameTW: String
    //var courseHours: String
    var destinationView: AnyView // 這裡使用 AnyView 表示任意視圖
    var id = UUID()
    //@State var trendNum :Int = 0
     
}

//struct Course:Identifiable{
//    var id = UUID()
//    var image:String
//    var courseNameUS:String
//    var courseNameTW:String
//    var courseHours:String
//}
var games = [
    GameInfo(image: "photo1", courseNameUS: "劉威佑", courseNameTW: "圈圈叉叉", destinationView: AnyView(ooxxView())),
    GameInfo(image: "IMG_0483", courseNameUS: "黃昱儒", courseNameTW: "翻牌遊戲",  destinationView: AnyView(flipCardFree())),
    GameInfo(image: "IMG_0447", courseNameUS: "詹旻嘉", courseNameTW: "貪食蛇",  destinationView: AnyView(snakeView())),
        GameInfo(image: "IMG_0484", courseNameUS: "王彥雅", courseNameTW: "數字炸彈",  destinationView: AnyView(FreeGameBomb())),
    GameInfo(image: "IMG_0485", courseNameUS: "陳姿蓉", courseNameTW: "水果消消樂",  destinationView: AnyView(FreeGameFruit())),
    GameInfo(image: "IMG_0486", courseNameUS: "王彥雅", courseNameTW: "1A2B",  destinationView: AnyView(oneAtwoBView())),
    GameInfo(image: "IMG_0487", courseNameUS: "許博翔", courseNameTW: "逃出月讀世界",  destinationView: AnyView(FreeGameBonk())),
    GameInfo(image: "IMG_0488", courseNameUS: "黃玟仁", courseNameTW: "接胡蘿蔔",  destinationView: AnyView(FreeGameCarrot())),
    
    // 添加其他遊戲信息
]
//
//var course = [
//    Course(image:"photo1",courseNameUS: "劉威佑",courseNameTW: "圈圈叉叉",courseHours: ""),
//    Course(image:"1A2B",courseNameUS: "王彥雅",courseNameTW: "1A2B",courseHours: ""),
//    Course(image:"flipCard",courseNameUS: "黃昱儒",courseNameTW: "翻牌遊戲",courseHours: ""),
//    Course(image:"candyCrash",courseNameUS: "陳姿蓉",courseNameTW: "水果消消樂",courseHours: ""),
//    Course(image:"snake",courseNameUS: "詹旻嘉",courseNameTW: "貪食蛇",courseHours: ""),
//    Course(image:"fruitCatch",courseNameUS: "黃玟仁",courseNameTW: "接水果",courseHours: ""),
//    Course(image:"snake",courseNameUS: "許博翔",courseNameTW: "打地鼠",courseHours: "")
//    
//]

//struct freeGameView: View {
//    
//    var body: some View {
//        NavigationView{
//            ScrollView{
//                ForEach(course){
//                    thisCourse in
//                    gameCardView(image:thisCourse.image,courseNameUS: thisCourse.courseNameUS,courseNameTW: thisCourse.courseNameTW,courseHours: thisCourse.courseHours)
//                }
//            }
//            .navigationTitle("自由模式")
//        }
//    }
//}

struct freeGameView: View {
    var body: some View {
        //ScrollView{
            NavigationView {
                ScrollView(.horizontal) {
                    HStack{
                        ForEach(games, id: \.courseNameTW) { game in
                            NavigationLink(
                                destination: {
                                    game.destinationView
                                },
                                label: {
                                    gameCardView(image: game.image, courseNameUS: game.courseNameUS, courseNameTW: game.courseNameTW )
                                }
                                
                            )
                            //.navigationBarTitleDisplayMode(.inline)
                            
                        }
                    }
            
             }
            .navigationTitle("Free Games")
                .frame(minWidth: 0, maxWidth: .infinity, minHeight: 0, maxHeight: .infinity)
            .background(Color(red:168/255,green:216/255,blue:130/255))
            }
            .navigationViewStyle(StackNavigationViewStyle())
      //  }
           

//        NavigationView {
//            ScrollView {
//                ForEach(games, id: \.courseNameUS) { game in
//                    NavigationLink(
//                        destination: game.destinationView,
//                        label: {
//                            gameCardView(image: game.image, courseNameUS: game.courseNameUS, courseNameTW: game.courseNameTW )
//                        }
//                    )
//                }
//            }
//            .navigationTitle("Free Games")
//        }
        
    }
}




    

      
 ```

  <img src="https://raw.githubusercontent.com/AmilyC/SwiftUI_final_project/main/IMG_0555.png">
     </td>
  </tr>
</table>
