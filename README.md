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
			for (Menu m : Menu.values()) { // 拡張for文
				System.out.printf("(%d) %s ", m.ordinal(), m.getMessage());
				// メニュー表示
				if ((m.ordinal() % 3) == 2 &&
					m.ordinal() != Menu.TERMINATE.ordinal())
					System.out.println(); // データ検索の後改行をいれるため（細かい）
			}
			System.out.print("："); // セパレータ
			key = stdIn.nextInt(); // メニュー選択の番号の読み込み
		} while (key < Menu.ADD.ordinal() || key > Menu.TERMINATE.ordinal());
		
		return Menu.MenuAt(key); // 選んだキーのメニューを返す
	}
	
	public static void main(String[] args) {
		Menu menu; // メニュー
		Data data; // 追加用データ参照
		Data temp = new Data(); // 読込み用データ
		
		ChainHash<Integer, Data> hash = new ChainHash<Integer, Data>(13);
		// チェインハッシュのインスタンス「hash」を生成，相称型（ジェネリック）を使っている．
		// <K,V>のそれぞれが，IntegerとDataになっていることに注意
		// 今回，ハッシュ票のサイズは13になっている．素数が良い．
		
		do {
			switch (menu = SelectMenu()) {
			case ADD : // 追加
				data = new Data();
				data.scanData("追加", Data.NO | Data.NAME);
				hash.add(data.keyCode(), data);
				break;
				
			case REMOVE : // 削除
				temp.scanData("削除", Data.NO);
				hash.remove(temp.keyCode());
				break;
				
			case SEARCH : // 探索
				temp.scanData("探索", Data.NO);
				Data t = hash.search(temp.keyCode());
				if (t != null)
					System.out.println("キーをもつデータは" + t + "です．");
				else
					System.out.println("キーはありません．");
				break;
				
			case DUMP : // 表示
				hash.dump();
				break;
			}
		} while (menu != Menu.TERMINATE);
	}
}
