# LibraryTestSystem
sample Library System 

### SystemLibrary.java

	package library.newBook;

	import java.util.ArrayList;
	import java.util.Scanner;
	import java.util.function.Consumer; 

	public class SystemLibrary {
		public ArrayList<Book> BookList = new ArrayList<Book>();
		public Scanner input = new Scanner(System.in);
		public SystemLibrary() {
		loadData();
    	}
	void systemlib() {
		System.out.print("請輸入選項數字(1~5)=>");
		String print = input.nextLine();
		while(true) {
	
			if(print.equals("1")) {
				System.out.print("請輸入查詢資訊:　a.編號 b.中文書名 c.英文書名 d.ISBN");
				input = new Scanner(System.in);
				String searchINFO = input.nextLine();
				if(searchINFO.equals("a")) {
					System.out.println("編號:  ");
					input = new Scanner(System.in);
					String NoName = input.nextLine();
					ArrayList<Book> resultList = findBooksByNoName(NoName);
					System.out.println("---------------------------------查詢結果--------------------------------");
	                if (!resultList.isEmpty()) {
	                	printBookList(resultList);
	                }
				}else if(searchINFO.equals("b")) {
					System.out.println("中文書名: ");	
				}else if(searchINFO.equals("c")){
					System.out.println("中文書名: ");
				}else if(searchINFO.equals("d")) {
					System.out.println("中文書名: ");
				}else {
					System.out.println("輸入錯誤，請重新輸入。");
				}
			
			
			}else if(print.equals("2")) {
				System.out.println("請輸入借閱書名: ");
				String NoName = input.nextLine();
				System.out.println("-----------------------------------------借閱結果---------------------------------");
                checkOutBook(NoName);
	      
	      
			}else if(print.equals("3")) {
				 System.out.print("請輸入 國際標準書號(ISBN)=>");
	             input = new Scanner(System.in, "big5");
	             String isbn = input.nextLine().toUpperCase().trim();
	             System.out.println("----------------------------------------------------還書結果-------------------------------");
	             returnBook(isbn);
	    
	    
			}else if(print.equals("4")) {
				System.out.print("請輸入 國際標準書號(ISBN),中文書名,英文書名,價格,數量(以逗點隔開)=>");
                input = new Scanner(System.in, "big5");
                String[] bookInfo = input.nextLine().toUpperCase().trim().split(",");
                System.out.println("-----------------------------------------捐書(新增)結果-------------------------------------------");
                if (bookInfo.length != 5) {
                    System.out.println("新增書籍的資訊不足!請重新操作");
                } else {
                    addBook(bookInfo);
                }
			}
			input.close();
		}
	}
	
## SystemLibrary 搭配部分
	//查詢
	private ArrayList<Book> findBooksByNoName(String bookNameANo) {
		ArrayList<Book> resultNumber = new ArrayList<Book>();
		for(Book b : BookList) {
			if(((String) b.numberB).startsWith(bookNameANo) || ((String) b.chineseB).startsWith(bookNameANo) || ((String) b.engB).startsWith(bookNameANo)) {
				resultNumber.add(b);
			}
		}
		return resultNumber;
	}
	private void printBookList(ArrayList<Book> BookList) {
		BookList.forEach(new Consumer<Book>(){
			int count = 1;

			public void accept(Book b) {
				System.out.println(count + "." + b.toString());
				count++;
			}
		});
	}
	
	//借閱
    private void checkOutBook(String name) {
    	
        Book book = queryByName(name);
        if (book != null) {
            if (book.onShelf > 0) {
                book.onShelf = book.onShelf - 1;
                System.out.println("借閱書名:" + book.chineseB + "(" + book.engB + ")" + " 數量:" + 1);
            } else {
                System.out.println(book.chineseB + "(" + book.engB + ") 已全部借出");
            }
        } else {
            System.out.println("書名沒有登記在本館目錄");
        }
    }
    private Book queryByName(String bookName) {
        Book book = BookList.stream()
                .filter(a -> a.chineseB.equals(bookName) || ((String) a.engB).equalsIgnoreCase(bookName))
                .findAny().orElse(null);
        return book;
    }
    
    //還書
    private void returnBook(String isbn) {
        Book book = queryByISBN(isbn);
        if (book != null) {
            book.onShelf++;
            System.out.println("歸還後架上數量：" + book.onShelf);
        } else {
            System.out.println("國際標準書號(ISBN)=" + isbn + " 沒有登記在本館目錄");
        }
    }
    private Book queryByISBN(String isbn) {
        Book book = BookList.stream()
                .filter(a -> a.isbn.equals(isbn)).findAny().orElse(null);
        return book;
    }
    
    //捐書(新增)
    private void addBook(String... bookInfo) {
    	Book book = new Book();
        boolean flag = false;
        for (Book b : BookList) {
            if (b.chineseB.equals(book.chineseB) && ((String) b.engB).equalsIgnoreCase(book.engB)) {
                b.onShelf++;
                System.out.println("新增書籍已存在!數量由" + (b.onShelf - 1) + "=>" + b.onShelf);
                System.out.println(b.toString());
                flag = true;
                break;
            }
        }
        if (flag == false) {
            BookList.add(book);
            System.out.println(book.toString());
        }
    }
    
    //倉庫裡面書籍
    private void loadData() {
        Book book1 = new Book("ABCD111111", "爪哇1", "java1", 450, 1);
        Book book2 = new Book("ABCD222222", "爪哇2", "java2", 450, 2);
        Book book3 = new Book("ABCD333333", "爪哇3", "java3", 450, 3);
        Book book4 = new Book("ABCD444444", "爪哇4", "java4", 450, 4);
        Book book5 = new Book("ABCD555555", "爪哇5", "java5", 450, 5);
        Book book6 = new Book("ABCD666666", "爪哇6", "java6", 450, 6);
        Book book7 = new Book("ABCD777777", "爪哇7", "java7", 450, 7);
        BookList.add(book1);
        BookList.add(book2);
        BookList.add(book3);
        BookList.add(book4);
        BookList.add(book5);
        BookList.add(book6);
        BookList.add(book7);
    }
    //Start first
	public void PrintMenu() {
		System.out.println("歡迎蒞臨 WC圖書館");
		System.out.println("1. 查詢");
		System.out.println("2. 借閱");
		System.out.println("3. 還書");
		System.out.println("4. 捐書");
		System.out.println("5. 退出系統");
	}
}

## Book.java
![image](https://user-images.githubusercontent.com/43702225/46254139-17164a80-c4bd-11e8-8f21-8c41be5e59e4.png)

	package library.newBook;
	public class Book {
		public Book(String string, String string2, String string3, int i, int j) {}
		public Book() {}
		public Object numberB;
		public Object chineseB;
		public String engB;
		public int onShelf;
		public Object isbn;
	}
	
## print 圖書館系統選項 Main.java
![image](https://user-images.githubusercontent.com/43702225/46254167-72483d00-c4bd-11e8-97ba-a8c89dd156ae.png)

	package library.newBook;
	public class Main {
		public static void main(String[] args) {
			SystemLibrary sysbooks = new SystemLibrary(); 
			sysbooks.PrintMenu();
			sysbooks.systemlib();//private
		}
	}
