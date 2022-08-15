# ScoreGenerator
## 這個 Repo 純粹當 cdn 用

## 使用方法
1. 確保你的 google sheet 為開放的，然後記下 fileID  
2. 將 <script src="https://cdn.jsdelivr.net/gh/t109598105/ScoreGenerator@latest/dist/scoregenerator.min.js"></script> 加入 html  


## JS 使用方法
範例 Code
`

    //範例程式碼請至 <字體設計與文字編碼> 課程的 HTML 檔案參考
	var GoogleSheetID = '<FILE_ID>';
	

    var cg2021f = new ScoreGenerator.CSVGenerator(GoogleSheetID);

    //指定一個 CORS 代理伺服器 
    cg2021f.corsProxy = '<CORS_SERVER>';
    
	//每呼叫一次即可加入不同的作業欄目
    cg2021f.addHwCustomCount({
		//欄目的前綴字串
		prefix: 'HW',
		//總共有幾次作業
		count: 4,
		//欄目的後綴字串
		postfix: '分數',
		//作業從第幾個編號開始
		startAt: 1
	});

    cg2021f.addHwCustomCount({
        prefix: 'HWP',
        count: 12,
        postfix: '分數',
        startAt: 1
    });

    //是哪一個學期，可以設定 fall 或 spring
    cg2021f.semester = 'spring';

    //學期開始的年份
    cg2021f.startYear = 2022;	
	
	var onLibInstalled = function(){

        //下載 google sheet
        cg2021f.downloadSheet(

            //下載 google sheet 完成後執行的 function
            function(){

                //將二進位資料讀取為 workbook
                cg2021f.mkSheet();

                //將 workbook 中所需的資料提取出來
                cg2021f.parseXLSX();

                //獲取成績表
                var score = cg2021f.exportScore();

                //獲取簽到單
                var attendanceSheet = cg2021f.exportAttendanceSheet();
                

                //以下為 d3.js 部分，data 為 d3.js 處理過的 CSV
                d3.csv(score.dataURI(), function(data){
					
                });


            document.getElementById('score-download').onclick = function(){ 
                score.download('cg2021f_成績');
            }

            document.getElementById('att-download').onclick = function(){ 
                attendanceSheet.download('cg2021f_簽到單'); 
            }

        }, 
        //google sheet 下載失敗後執行的 function
        function(){
            console.log('Failed to download sheet');
        });
    };
	
	//確認是否安裝依賴函式庫
    if(!cg2021f.libInstalled){

        //安裝依賴函式庫
        cg2021f.installLibrary(function(){
            onLibInstalled();
        }, 
        //安裝依賴函式庫失敗後執行的 function
        function(){
            console.error('Library is not installed');
        });
    }
`

## 程式問題
如果有程式上的問題或是有錯誤需要修正  
請直接聯繫我的[電子信箱](mailto:poynt2005@gmail.com)
	


