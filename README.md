# ChainHashMain2
チェイン法によるハッシュ2

import java.util.Scanner;
public class ChainHashMain {
	static Scanner stdIn = new Scanner(System.in);
	
	static class Data {
		static final int NO = 1; // 番号を読み込むか？
		static final int NAME = 2; // 氏名を読み込むか？
		
		private Integer no; // 会員番号（キー値）
		private String name; // 氏名
		
		Integer keyCode() {
			return no;
		}
		
		public String toString() {
			return name;
		}
		
		void scanData(String guide, int sw) { // データの読み込み
			System.out.println(guide + "するデータを入力してください．");
			// 見出し
			if ((sw & NO) == NO) { //　先に願号を入れさせて
				System.out.print("番号：");
				no = stdIn.nextInt(); // 番号の読みとり
			}
			if ((sw & NAME) == NAME) { // 次に名前を入れさせる
				System.out.print("名前：");
				name = stdIn.next(); // 名前の読みとり
			}
		}
	}
	
	enum Menu { // 列挙型のメニュー
		ADD("データ追加"),
		REMOVE("データ削除"),
		SEARCH("データ探索"),
		DUMP("全データ表示"),
		TERMINATE("終了");
		
		private String message;
		
		static Menu MenuAt(int idx) { // idxの列挙を返す
			for (Menu m : Menu.values())
				if (m.ordinal() == idx)
					return m;
			return null;
		}
		
		//ここから
Menu(String string) { // コンストラクタ
message = string;
}
String getMessage() { // 表示用文字列を返す
return message;
}
}
static Menu SelectMenu() { // メニューの選択，メニュー表示と合わせて
int key;
do {
