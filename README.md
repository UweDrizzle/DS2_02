#include <stdio.h> // 10727231 林曉凡 10727235 葉育瑋 
#include <stdlib.h>
#include <stdbool.h>
#include <cstdio>
#include <string.h>
#include <math.h> 
#include <fstream>
#include <iostream>
#include <vector>
#include <time.h>
using namespace std ;

struct Data{
	string name,departmentname,category,level,city,system;
	string student,teacher, code ,  departmentcode ;
	int graduate ;	
	int number ;
};

struct Node{
	vector<Data> school[2];
	vector<Data> school2 ;
	Node *right;
	Node *right2;
	Node *left;
	Node *left2 ;
	Node *mid ;
	Node *mid2 ;
	Node *father ;
	Node *n1 ;
	Node *n2 ;
	Node *n3 ;
	Node *n4 ;
	int Nodenumber ;
	int max ;
	int min ;
};




class Read{
private:
	vector<Data> data;

	Data o ;

	
	string filenum ;
	int DataSize ;
public:
	
	
	
	
	Data D( string str ){
		int heapsize = 0 ;
		Data o ;
		char c = '\0' ;
		int i = 0 ;
		string str2 ;
		int t = 0 ;
		bool b = false ;
		str2 = "" ;
		bool tab = false ;
		o.number = DataSize + 1 ;
		while( str[i] != '\0' ) {
		
			if( str[i] == '\t' ) {
				t++ ;
			}
			if( str[i] != '\t' ) {
				//str2 = str2 + str[i] ;
				while( !tab && str[i] != '\0' ){
					if( str[i] != '\t' ){
						str2 = str2 + str[i] ;
						i++ ;
					}
					else {
						tab = true ;
						if( t == 1 )
							o.name = str2 ;
						else if( t == 3 )
							o.departmentname = str2 ;
						else if( t == 4 )
							o.category = str2 ;
						else if( t == 5 )
							o.level = str2 ;
						else if( t == 6 )
							o.student = str2 ;
						else if( t == 7 )
							o.teacher = str2 ;
						else if( t == 8 ) {
							o.graduate = atoi( str2.c_str()) ;
							b = true ;
							break ;
						}
						t++ ;
						str2 = "" ;
					}
				}
				
			}
			if( str[i] != '\0' ) 
				i++ ;
			tab = false ;
			if( b )
				break ;
			
		}

		return o ;
		
	}
	
	bool InputData( int h ){    // 將資料檔存放到動態陣列裡 
		string filename ;
		FILE *file ;
		int  n;
		DataSize = 0 ;
		
		cout << "請輸入檔案名稱\n" ; 
		cin >> filename ;
		filenum=filename;
		if ( h == 1 ) 
  			filename = "input" + filename + ".txt" ;
        //file.open( filename.c_str() ) ;
        file = fopen( filename.c_str(), "r" ) ;
        
        
  		if ( file == NULL ) { 
  			return false ;
  		} 
  		else { 
  			char CNDDCLCS[20] ;
  			char c = '\0' ;
  			
  			
  			for( int i = 1 ; i <=4 ; i++)  // 讀一開始的東西 
  				fscanf( file, "%s", CNDDCLCS) ;			
  			
  			for(int i=1;i<=11 ;i++){ // 讀一開始的東西 
  				fscanf( file, "%s", CNDDCLCS) ;			
			}			
			
			//cout << "\n";     
			string str1 ;
			string str2 ;
			string skip ;
			string str ;
			
			int graduate ;
			
			bool one = true ;
			//fscanf( file, "%c", &c ) ;
			while( fscanf( file, "%c", &c ) != EOF ) {
				str = "" ;
				if ( !one )
					str = str+c ;
				c = '\0' ;
				while( c != '\n' && fscanf( file, "%c", &c ) != EOF ) {
					if( c != '\n' )
						str = str + c ;
					
				}
				
				one = false ;
				o = D( str ) ;

    			data.push_back( o ) ;
    			DataSize++ ;
    			
				
			}
			   	
			fclose( file ) ;	
       	
       		return true;
		}		
		return false ;	
	}
	
    
	void deletevector(){ //清空VECTOR 
		data.clear() ;
	
	}
	
	vector<Data> R(){
		if( InputData(1) ){
			return data ;
		}
		else 
			cout << "ERROR" ;
	}

	
	
};




class Two3tree{
	private:
		vector<Data> tree ;
		Node *root ;
		Node *q ;
	public:
		void doit( vector<Data> data ){
			root = NULL ;
			
			
			
			
			for( int i = 0 ; i < data.size() ; i++ ){
				
				InsertItem( data[i], root ) ;
		//		cout << data[i].name << '\n' ;
			}
			cout << root->school[0][0].name;
			
			cout << root->left->school[0][0].name;
			cout << root->right->school[0][0].name;
			cout << FH( root ) ;
			//if( root !=NULL )
			//cout << "12321312312" ;
			
			//cout<< root->school[0][0].name ;
			//cout << root->left->school[0][0].name << '\n' << root->school[0][0].name <<'\n' << root->right->school[0][0].name ;
		}
		
		
		void InsertItem( Data data, Node *head ) {
			
			if( root == NULL ){
				Node *temp = new Node();
				temp->school[0].push_back( data ) ;
				temp->Nodenumber = 1 ;

				temp->father = NULL ; 
				temp->left = NULL ;
				temp->right = NULL ;
				temp->mid = NULL ;
				temp->n1 = NULL ;
				temp->n2 = NULL ;
				temp->n3 = NULL ;
				temp->n4 = NULL ;
				root = temp ;
			}
			else{
				if( head->left == NULL && head->mid == NULL && head->right == NULL ) {  
					if( head->Nodenumber != 2 ) {
						if( data.name > head->school[0][0].name ){
							q = head ;
							q->school[1].push_back( data ) ;
							q->max = 1 ;
							q->min = 0 ;
							q->Nodenumber = 2 ;
						}
						else if( data.name < head->school[0][0].name ){
							q = head ;
							q->school[1].push_back( data ) ;
							q->max = 0 ;
							q->min = 1 ;
							q->Nodenumber = 2 ;
						}
						else if( data.name == head->school[0][0].name ){
							q = head ;
							q->school[0].push_back( data ) ;
						}
					}
					else if( head->Nodenumber == 2 ){
						vector<Data> datas ;
						datas.push_back( data ) ;
						head->school2 = datas ;
						head->Nodenumber = 3 ;
						split( head ) ;
					}
				}
				else{
					if( head->Nodenumber == 1 ) {
						if( data.name > head->school[0][0].name ){
							q = head ;
							q->right->father = head ;
							InsertItem( data, head->right ) ;
						} 
						else if( data.name < head->school[0][0].name ){
							q = head ;
							q->left->father = head ;
							InsertItem( data, head->left ) ;
						} 
					}
					else if( head->Nodenumber == 2 ) {
						if( data.name > head->school[head->max][0].name ){
							q = head ;
							q->right->father = head ;
							InsertItem( data, head->right ) ;
						}
						else if( data.name < head->school[head->min][0].name ){
							q = head ;
							q->left->father = head ;
							InsertItem( data, head->left ) ;
						}
						else if( data.name < head->school[head->max][0].name && data.name > head->school[head->min][0].name ){
							q = head ;
							q->mid->father = head ;
							InsertItem( data, head->mid ) ;
						}
					}
					else if( data.name == head->school[0][0].name ){
						q = head ;
						q->school[0].push_back( data ) ;
					}	
					
				}	
				
			}
			
			
		}
		
		
	
	
		Node MakeNode( vector<Data> data ){
			Node *temp = new Node() ;
			temp->school[0] = data  ;
			temp->Nodenumber = 1 ;
			return *temp ;
		}
	
	
		void split( Node *head ){
			if( head->father = NULL ) {  // ROOT
				if( head->left == NULL && head->mid == NULL && head->right == NULL ){
					if( head->school2[0].name > head->school[head->max][0].name ) {// data = max
						Node *left = new Node() ;
						left->school[0] = head->school[head->min]  ;
						left->Nodenumber = 1 ;
						
						Node *right = new Node() ;
						right->school[0] = head->school2 ;
						right->Nodenumber = 1 ;
						
						Node *newhead = new Node() ;
						newhead->school[0] = head->school[head->max]  ;
						newhead->Nodenumber = 1 ;						
						
						newhead->left = left;
						newhead->left->father = newhead ;
						newhead->right = right;
						newhead->right->father = newhead ;
						head = newhead ;
					}
					else if( head->school2[0].name < head->school[head->min][0].name ) {// data = min
						Node *left = new Node() ;
						left->school[0] = head->school2  ;
						left->Nodenumber = 1 ;
						
						Node *right = new Node() ;
						right->school[0] = head->school[head->max]  ;
						right->Nodenumber = 1 ;
						
						Node *newhead = new Node() ;
						newhead->school[0] = head->school[head->min]  ;
						newhead->Nodenumber = 1 ;	
											
						newhead->left = left;
						newhead->left->father = newhead ;
						newhead->right = right;
						newhead->right->father = newhead ;
						head = newhead ;
					}
					else if( head->school2[0].name < head->school[head->max][0].name && head->school2[0].name > head->school[head->min][0].name ) { // data = mid
						Node *left = new Node() ;
						left->school[0] = head->school[head->min]  ;
						left->Nodenumber = 1 ;
						
						Node *right = new Node() ;
						right->school[0] = head->school[head->max]  ;
						right->Nodenumber = 1 ;
						
						Node *newhead = new Node() ;
						newhead->school[0] = head->school2  ;
						newhead->Nodenumber = 1 ;						
						
						newhead->left = left;
						newhead->left->father = newhead ;
						newhead->right = right;
						newhead->right->father = newhead ;
						head = newhead ;
					}
					
				}
				else{
					if( head->school2[0].name > head->school[head->max][0].name ) {// data = max
						Node *left = new Node() ;
						left->school[0] = head->school[head->min]  ;
						left->Nodenumber = 1 ;
						
						Node *right = new Node() ;
						right->school[0] = head->school2 ;
						right->Nodenumber = 1 ;
						
						Node *newhead = new Node() ;
						newhead->school[0] = head->school[head->max]  ;
						newhead->Nodenumber = 1 ;						
						
						newhead->left = left;
						newhead->left->father = newhead ;
						newhead->right = right;
						newhead->right->father = newhead ;
						newhead->left->left = head->n1 ;
						newhead->left->left->father = newhead->left ;
						newhead->left->right = head->n2 ;
						newhead->left->right->father = newhead->left ;
						newhead->right->left = head->n3 ;
						newhead->right->left->father = newhead->right ;
						newhead->right->right = head->n4 ;
						newhead->right->right->father = newhead->right ;
						head = newhead ;
					}
					else if( head->school2[0].name < head->school[head->min][0].name ) {// data = min
						Node *left = new Node() ;
						left->school[0] = head->school2  ;
						left->Nodenumber = 1 ;
						
						Node *right = new Node() ;
						right->school[0] = head->school[head->max]  ;
						right->Nodenumber = 1 ;
						
						Node *newhead = new Node() ;
						newhead->school[0] = head->school[head->min]  ;
						newhead->Nodenumber = 1 ;	
											
						newhead->left = left;
						newhead->left->father = newhead ;
						newhead->right = right;
						newhead->right->father = newhead ;
						newhead->left->left = head->n1 ;
						newhead->left->left->father = newhead->left ;
						newhead->left->right = head->n2 ;
						newhead->left->right->father = newhead->left ;
						newhead->right->left = head->n3 ;
						newhead->right->left->father = newhead->right ;
						newhead->right->right = head->n4 ;
						newhead->right->right->father = newhead->right ;
						head = newhead ;
					}
					else if( head->school2[0].name < head->school[head->max][0].name && head->school2[0].name > head->school[head->min][0].name ) { // data = mid
						Node *left = new Node() ;
						left->school[0] = head->school[head->min]  ;
						left->Nodenumber = 1 ;
						
						Node *right = new Node() ;
						right->school[0] = head->school[head->max]  ;
						right->Nodenumber = 1 ;
						
						Node *newhead = new Node() ;
						newhead->school[0] = head->school2  ;
						newhead->Nodenumber = 1 ;	
											
						newhead->left = left;
						newhead->left->father = newhead ;
						newhead->right = right;
						newhead->right->father = newhead ;
						newhead->left->left = head->n1 ;
						newhead->left->left->father = newhead->left ;
						newhead->left->right = head->n2 ;
						newhead->left->right->father = newhead->left ;
						newhead->right->left = head->n3 ;
						newhead->right->left->father = newhead->right ;
						newhead->right->right = head->n4 ;
						newhead->right->right->father = newhead->right ;
						head = newhead ;
					}
					
				}
			}
			else{
				if( head->father->Nodenumber == 1 ) { 
					if( head->school[head->max][0].name < head->father->school[0][0].name ){//left full
						if( head->school2[0].name > head->school[head->max][0].name ) {// data = max
							Node *left = new Node() ;
							left->school[0] = head->school[head->min]  ;
							left->Nodenumber = 1 ;
							left->father = head->father ;
						
							Node *mid = new Node() ;
							mid->school[0] = head->school2 ;
							mid->Nodenumber = 1 ;
							mid->father = head->father ;
						
							head->father->left = left ;
							head->father->mid = mid ;
						
							head->father->school[1] = head->school[head->max]  ;
							head->father->Nodenumber = 2 ;						
							if( head->father->school[0][0].name > head->father->school[1][0].name ){
								head->father->max = 0 ;
								head->father->min = 1 ;
							}
							else{
								head->father->max = 1 ;
								head->father->min = 0 ;
							}
						}
						else if( head->school2[0].name < head->school[head->min][0].name ) {// data = min
							Node *left = new Node() ;
							left->school[0] = head->school2  ;
							left->Nodenumber = 1 ;
							left->father = head->father ;
						
							Node *mid = new Node() ;
							mid->school[0] = head->school[head->max] ;
							mid->Nodenumber = 1 ;
							mid->father = head->father ;
						
							head->father->left = left ;
							head->father->mid = mid ;
						
							head->father->school[1] = head->school[head->min]  ;
							head->father->Nodenumber = 2 ;						
							if( head->father->school[0][0].name > head->father->school[1][0].name ){
								head->father->max = 0 ;
								head->father->min = 1 ;
							}
							else{
								head->father->max = 1 ;
								head->father->min = 0 ;
							}
						}
						else if( head->school2[0].name < head->school[head->max][0].name && head->school2[0].name > head->school[head->min][0].name ) { // data = mid
							Node *left = new Node() ;
							left->school[0] = head->school[head->min]  ;
							left->Nodenumber = 1 ;
							left->father = head->father ;
						
							Node *mid = new Node() ;
							mid->school[0] = head->school[head->max] ;
							mid->Nodenumber = 1 ;
							mid->father = head->father ;
						
							head->father->left = left ;
							head->father->mid = mid ;
						
							head->father->school[1] = head->school2  ;
							head->father->Nodenumber = 2 ;						
							if( head->father->school[0][0].name > head->father->school[1][0].name ){
								head->father->max = 0 ;
								head->father->min = 1 ;
							}
							else{
								head->father->max = 1 ;
								head->father->min = 0 ;
							}
						}
					
					}
					else if( head->school[head->min][0].name > head->father->school[0][0].name ) {  // right full
						if( head->school2[0].name > head->school[head->max][0].name ) { // data = max
							Node *right = new Node() ;
							right->school[0] = head->school2  ;
							right->Nodenumber = 1 ;
							right->father = head->father ;
						
							Node *mid = new Node() ;
							mid->school[0] = head->school[head->min] ;
							mid->Nodenumber = 1 ;
							mid->father = head->father ;
						
							head->father->right = right ;
							head->father->mid = mid ;
						
							head->father->school[1] = head->school[head->max]  ;
							head->father->Nodenumber = 2 ;						
							if( head->father->school[0][0].name > head->father->school[1][0].name ){
								head->father->max = 0 ;
								head->father->min = 1 ;
							}
							else{
								head->father->max = 1 ;
								head->father->min = 0 ;
							}
						}
						else if( head->school2[0].name < head->school[head->min][0].name ) {// data = min
							Node *right = new Node() ;
							right->school[0] = head->school[head->max]  ;
							right->Nodenumber = 1 ;
							right->father = head->father ;
						
							Node *mid = new Node() ;
							mid->school[0] = head->school2 ;
							mid->Nodenumber = 1 ;
							mid->father = head->father ;
						
							head->father->right = right ;
							head->father->mid = mid ;
						
							head->father->school[1] = head->school[head->min]  ;
							head->father->Nodenumber = 2 ;						
							if( head->father->school[0][0].name > head->father->school[1][0].name ){
								head->father->max = 0 ;
								head->father->min = 1 ;
							}
							else{
								head->father->max = 1 ;
								head->father->min = 0 ;
							}
						}
						else if( head->school2[0].name < head->school[head->max][0].name && head->school2[0].name > head->school[head->min][0].name ) { // data = mid
							Node *right = new Node() ;
							right->school[0] = head->school[head->max]  ;
							right->Nodenumber = 1 ;
							right->father = head->father ;
						
							Node *mid = new Node() ;
							mid->school[0] = head->school[head->min] ;
							mid->Nodenumber = 1 ;
							mid->father = head->father ;
						
							head->father->right = right ;
							head->father->mid = mid ;
						
							head->father->school[1] = head->school2  ;
							head->father->Nodenumber = 2 ;						
							if( head->father->school[0][0].name > head->father->school[1][0].name ){
								head->father->max = 0 ;
								head->father->min = 1 ;
							}
							else{
								head->father->max = 1 ;
								head->father->min = 0 ;
							}
						}
					}
					
					
				}
				else if( head->father->Nodenumber == 2 ) { ////////////////////////寫到這裡如果父節點有兩個 
					;
				}
			}
			
		}
	
	
	
		int Max( int a,int b,int c){
			if( a >= b && a >=c )
				return a ;
			else if ( b >= a && b >= c )
				return b ;
			else if ( c >= a && c >= b )
				return c ;
		}
	
		int FH( Node *head ){
			if( head == NULL )
				return 0 ;
			return Max( FH( head->left ), FH( head->right ), FH( head->mid ) ) +1 ;
		}
	
	
	
	
};

void m1(){
	vector<Data> data ;
	Node *root = NULL;
	Read a ;
	data = a.R() ;
	Two3tree b ;
	b.doit( data ) ;
}




int RC(){ // 判別輸入值
	char str[]={0} ;
	scanf( "%s", str ) ;
	unsigned long long int i ;
	unsigned long long int num;
	for(i = 0; i <= strlen(str)-1 ; i++){
		if ( str[i]-'0'>9 || str[i]-'0'<0 ){
			return -99999 ;
		}
	}
	num = atoi(str);

	return num ;
}

int main(){
	
	
	int com = -1 ;
	printf( "請選擇要執行的指令\n" ) ; 
	printf( "0 結束\n1 min heap\n2 MinMaxHeap\n" ) ;
	com = RC() ;
	while (  com > 3 || com < 0 ) {
		printf( "指令錯誤  請重新輸入\n" ) ;
		com = RC() ;
	}
	while( com == 0 || com == 1 || com ==2 || com == 3) {
		if ( com == 1 || com == 2 || com == 3) {
			if ( com == 1 ) {
				m1() ;
				//Mission1() ;  
			}
			else if ( com == 2 ) 
				//Mission2() ;  
		 
				 
			
			printf( "請選擇要執行的指令\n" ) ;
			printf( "0 結束\n1 min heap\n2 MinMaxHeap\n" ) ;
			com = RC() ;
			while (  com > 3 || com < 0 ) {
				printf( "指令錯誤  請重新輸入\n" ) ;
				com = RC() ;
			}
		}
		else if ( com == 0 ) {
			printf( "結束程式" ) ;
			com = 4 ;
		} 
	}

	return 0 ;	
}
