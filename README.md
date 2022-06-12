# 할 일 목록 앱
테이블 뷰 컨트롤러를 이용하여 목록을 추가 제거하며 페이지를 이동할 수 있는 앱입니다.\
iOS 13 Pro를 기준으로 제작했습니다.

<img src="https://user-images.githubusercontent.com/105588287/173221623-8ed9c5e4-0b67-47d5-8420-131d43ac0427.png" width="350" height="700"/> <img src="https://user-images.githubusercontent.com/105588287/173221803-6febfe10-cdcc-4992-a1eb-b4aae827844c.png" width="350" height="700"/>

# 환경
cord
# Source
**목록보여주기**\
섹션의 개수와 섹션당 열의 개수 설정


    override func numberOfSections(in tableView: UITableView) -> Int {
        // #warning Incomplete implementation, return the number of sections
        return 1
    }

    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        // #warning Incomplete implementation, return the number of rows
        return items.count
    }

목록과 목록에 들어가는 이미지의 값을 셀에 삽입

    
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "myCell", for: indexPath)

        cell.textLabel?.text = items[(indexPath as NSIndexPath).row]
        cell.imageView?.image = UIImage(named: itemsImageFile[(indexPath as NSIndexPath).row])

        return cell
 **목록삭제**
 
    override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCell.EditingStyle, forRowAt indexPath: IndexPath) {
        if editingStyle == .delete {
            // Delete the row from the data source
            items.remove(at: (indexPath as NSIndexPath).row)
            itemsImageFile.remove(at: (indexPath as NSIndexPath).row)
            tableView.deleteRows(at: [indexPath], with: .fade)
        } else if editingStyle == .insert {
            // Create a new instance of the appropriate class, insert it into the array, and add a new row to the table view
        }
    }
    
    //Delete를 삭제로 변경
    override func tableView(_ tableView: UITableView, titleForDeleteConfirmationButtonForRowAt indexPath: IndexPath) -> String? {
        return "삭제"
    }
    
**목록추가**

        items.append(tfAddItem.text!)
        itemsImageFile.append("clock.png")
        tfAddItem.text=""
        _ = navigationController?.popViewController(animated: true)
        
추가된 내용 호출

    override func viewWillAppear(_ animated: Bool) {
            tvListView.reloadData()
    }

**목록순서변경**

    // Override to support rearranging the table view.
    override func tableView(_ tableView: UITableView, moveRowAt fromIndexPath: IndexPath, to: IndexPath) {
        let itemToMove = items[(fromIndexPath as NSIndexPath).row]
        let itemImageToMove = itemsImageFile[(fromIndexPath as NSIndexPath).row]
        items.remove(at: (fromIndexPath as NSIndexPath).row)
        itemsImageFile.remove(at: (fromIndexPath as NSIndexPath).row)
        items.insert(itemToMove, at: (to as NSIndexPath).row)
        itemsImageFile.insert(itemImageToMove, at: (to as NSIndexPath).row)
    }

**세부내용화면**\
세그웨이를 이용하여 뷰를 전환

    // In a storyboard-based application, you will often want to do a little preparation before navigation
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        // Get the new view controller using segue.destination.
        // Pass the selected object to the new view controller.
        if segue.identifier == "sgDetail" {
            let cell = sender as! UITableViewCell
            let indexPath = self.tvListView.indexPath(for: cell)
            let detailView = segue.destination as! DetailViewController
            detailView.reciveItem(items[((indexPath! as NSIndexPath).row)])
        }
    }
    
