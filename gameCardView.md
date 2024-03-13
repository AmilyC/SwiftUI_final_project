 <h1>gameCardView</h1>
<h2>遊戲像卡片一樣呈現東西</h2>
<table>
  <tr>
    
    
      
 ```swift
  
import SwiftUI

struct gameCardView:View{
    var image:String
    var courseNameUS:String
    var courseNameTW:String
    //var courseHours:String
    var body:some View{
       // NavigationLink(destination: :ooxxView()){
            VStack{
                Image(image)
                    .resizable()
//                    .scaledToFit()
                    .aspectRatio(contentMode: .fill)
                    .frame(minWidth:0, maxWidth: 400, minHeight: 0, maxHeight: 500)
                VStack(alignment: .leading, content: {
                    Text(courseNameUS)
                        .font(.headline)
                        .foregroundStyle(.secondary)
                        .foregroundColor(.white)
                    Text(courseNameTW)
                        .font(.title)
                        .foregroundStyle(.primary)
                        .foregroundColor(.white)
                        .lineLimit(3)
//                    Text(courseHours)
//                        .font(.caption)
//                        .foregroundStyle(.purple)
                })
                .frame(minWidth:0, maxWidth: .infinity,alignment: .leading)
                .padding(.horizontal,15)
                .padding(.bottom,10)
            }
            .background(Color(red:220/255,green:92/255,blue:47/255))
            .clipShape(.rect(cornerRadius: 20))
            .overlay(
                RoundedRectangle(cornerRadius: 20)
                    .stroke(Color(red:255/255,green:157/255,blue:138/255), lineWidth: 2)
            )
            .padding(.all,10)
            //
            //        .onTapGesture{
            //            NavigationLink(destination: ooxxView()){
            //                EmptyView()
            //            }
            //            .hidden()
            //        }
            //
  //      }
    }
}




    

      
 ```

  <img src="https://raw.githubusercontent.com/AmilyC/Yzu-swiftui/main/Hw1.png">
     </td>
  </tr>
</table>
