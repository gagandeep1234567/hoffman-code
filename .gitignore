
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define NODE(arr) ((arr)->u.ad.frequency)
#define ADDRESS(arr) ((arr)->u.address->data.frequency)
#define FLAG(arr) ((arr)->flag)
#define macro 10

typedef struct node	//Structure of Client    
{
	char id;
	int frequency;
}node;

typedef struct treenode  // Structure of Tree        
{
	node data;
	struct treenode *left;
	struct treenode *right;
}treenode;

typedef struct linknode	// Stucture of Node         
{
	int flag;
	union option
	{
	      node ad;
	      treenode *address;
	}u;
	struct node *next;
}linknode;

typedef struct hashmap 	// Stucture of Hashmap
{
	char *code;
}hashmap;


linknode* new_node()	// function creation of node
{
	linknode *newnode = (linknode *)malloc(sizeof(linknode));
	newnode->flag = 0 ;
	return newnode;
}

treenode *createtree()	// function creation of Tree
{
	treenode *newnode = (treenode *)malloc(sizeof(treenode));
	newnode->left = NULL;
	newnode->right = NULL ;
	return newnode;	
}

linknode *push(linknode *head,node arr) // function for Insertion
{
	linknode *temp = new_node();
	temp->u.ad = arr;
	temp->next = head;
	head = temp;
	return head;
}

void heapify(node *a,int n)
{
        int i,j;
        node temp;

	j = 0;
	temp = a[j];

	i = 2*j+1;
	while(i<=n-1) 
	{
		if(i+1 <= n-1)
		
		   if(a[i].frequency <a[i+1].frequency)
		    i++;
		    
		   if(temp.frequency<a[i].frequency) 
		   {
			a[j]= a[i];
		        j = i;
			i = 2*j+1;
		   } 
		  else
		   break;
	}
	a[j] = temp;
	
}
void heap_sort(node *arr,int n)
{
     int j;
     node temp;
     for(j=n-1;j>0; j--)
     {  
         temp=arr[j];
         arr[j]=arr[0];
         arr[0]=temp;
         
         heapify(arr,j);     
     }
}

void max_heap(node *arr ,int n) //FUNCTION OF MAX HEAP
{
   int c,p,i,t,l;
  
   for(i=1;i<n;i++)
   {
     c=i;
     p=(c-1)/2;
     t=arr[c].frequency;
     l=arr[c].id;
     while(t > arr[p].frequency && c >0)
     {
        arr[c].frequency=arr[p].frequency;
        arr[c].id=arr[p].id;
        c=p;
        p=(c-1)/2;
     }
     
     arr[c].frequency=t;
     arr[c].id=l; 
         
   }
}

void display(linknode *head)	// FUNCTION FOR DISPLYING NODE
{
	linknode *current ;
	current=head;
	 printf("\t ID  \t  FREQ \n ");
	 
	while(current != NULL)
	{
		printf("\t  %c \t", current->u.ad.id );
		printf("  %d \n", current->u.ad.frequency);
		current = current->next;
	}
}

linknode* input_sort(node *arr , int n , linknode *head ,int *check) // FUNCTION FOR ACCEPTING INPUT 
{
	char g;
	for (int i = 0; i < n; ++i)
	{
	        printf("enter the  %d ID:\n",i+1);
		scanf("%c",&arr[i].id);
		printf("enter the  %d frequency:\n",i+1);
		scanf("%d",&arr[i].frequency);
		scanf("%c",&g);
		
		check[arr[i].id - 'a']++;
		
		if(check[arr[i].id - 'a'] > 1)//check for duplicate Id
		{
			printf("\nYou Entered Duplicate ID \n");
			return 0;
		}
	}
	
	max_heap(arr,n);
	
	heap_sort(arr,n); //calling of heap sort
	
	head = new_node();
	
	head->u.ad = arr[n-1];
	
	head->next = NULL;
	
	for (int i = n-2; i >= 0; i--)
	{
		head = push(head,arr[i]);
	}
	display(head);
	
	free(arr);
	return head;
}

void encoding(treenode *root,int index,char ch[],hashmap map[])		// FUNCTION FOR ENCODING
{
	if(root != NULL)
	{
		int i;
		if(root->left == NULL)
		{
			map[root->data.id - 'a'].code = (char *)malloc(sizeof(char)*(index ));
			ch[index] = '\0';
			strcpy(map[root->data.id - 'a'].code,ch);
		}
		ch[index] = '0';
		
		encoding(root->left,index+1,ch,map);
		
		ch[index] = '1';
		
		encoding(root->right,index+1,ch,map);
	}
}


treenode* build_tree(linknode *head)	// Function for Building Tree
{
	treenode *lc = NULL; 
	while(1)
	{
		treenode  *rc,*parent;
		linknode *temp = head;
		if (FLAG(temp) == 0)
		{
			lc = createtree();
			lc->data = temp->u.ad;
		}
		else
		{
			lc = temp->u.address; 
		}
		head = head->next;
		free(temp);
		if(head == NULL)
		{
			return lc;
		}
		temp = head;
		if (FLAG(temp) == 0)
		{
			rc = createtree();
			rc->data = temp->u.ad;
		}
		else
		{
			rc = temp->u.address; 
		}
		head = head->next;
		
		parent = createtree();
		
		parent->data.frequency = lc->data.frequency + rc->data.frequency;
		parent->left = lc ;
		parent->right = rc ;
		FLAG(temp) = 1 ;
		
		temp->u.address = parent;
		
		int pat = parent->data.frequency;

		if(head == NULL || (FLAG(head) == 0 && NODE(head) >= pat) || (FLAG(head) == 1 && ADDRESS(head) >= pat))
		{
			temp->next = head;
			head = temp;
		}
		else
		{
			linknode *curr = head,*prev;
			
			while(curr != NULL && (	(FLAG(curr) == 0 &&  NODE(curr) < pat ) || (FLAG(curr) == 1 && ADDRESS(curr) < pat)))
			{
				prev = curr;
				curr = curr->next;
			}
			temp->next = prev->next;
			prev->next = temp;
		}
	}
	return lc;
}


int main()
{
	
	int check[26];
	int n,sw;
	char g,ch[500],string[500]; 
	linknode *head = NULL;
	treenode *root = NULL;
	hashmap str[26];
	  for(int i = 0 ; i <  26 ; i++)
	  {
	     check[i] = NULL;
	  }	
	do
	{
		printf("\n\n\n1.ENTER INPUT \n2.BUILD TREE \n3.ENCODING \n4.DECODING \n5 TERMINATE\n");
		printf("ENTER THE CHOICE:: ");
		scanf("%d",&sw);
		switch(sw)
		{
			case 1 :	
			
						printf("Enter the no.of node\n");
						scanf("%d",&n);
						node *arr = (node *)malloc(sizeof(node) * (n));
						scanf("%c",&g);
						head = input_sort(arr,n,head,check);//shorting the number
						break;
						
						
			case 2 :	      if(head != NULL)
						{
						       
						
							for(int i = 0 ; i <  26 ; i++)
							{
								str[i].code = NULL;
							}
													
							
							root = build_tree(head);
							encoding(root,0,ch,str);
							
							printf("\nTREE IS SUCCESSFULLY CREATED\n");
						}
						else
							printf("\nPLEASE ENTER THE INPUT FIRST \n");	
						break;
			case 3 :    if(root != NULL)
						{
							int i;
							printf("ENTER THE STRING : ");
							scanf("%s",string);
							for(i = 0 ; string[i]!= '\0' ; i++)
							{
								if(check[string[i]- 'a'] < 1)
								{
									printf("INVALID STRING\n");
									break;
								}
								
								
							}
							if(string[i] == '\0')
							{
							
							      printf("\n\n\n\nEQUVALENT HOFFMAN CODE IS: ");
								for(i = 0 ; string[i]!= '\0' ; i++)
								{
									printf(" %s ", str[string[i]- 'a'].code);
								}
								printf("\n\n");		
							}

						}
						else
							printf("PLEASE BUILD THE TREE FIRST \n");
						break;

			case 4 :    if(root != NULL)
						{
							int i = 0,flag = 0;
							printf("ENTER THE STRING : ");
							scanf("%s",string);
							
							int d = strlen(string);//finding the length
							
							for (i = 0; i < d; ++i)
							{
								if(string[i] != '0' && string[i] != '1')
								{
									printf("INVALID STRING\n");
									break;
								}
							}
							
							if( i == d )//check all the value to be decoded
							{
								treenode *temp = root;
								i = 0 ;
								
								printf("\nEQUVALENT ID IS : ");
								while(string[i] != '\0')
								{
									if(string[i] != '0' && string[i] != '1')
									{
										printf("INVALID STRING\n");
										flag = 1;
										break;
									}
									if(temp->left == NULL)
									{
									        
										printf(" %c ",temp->data.id);
										temp = root;
									}
									else if(string[i] == '0')
									{
										temp = temp->left;
										i++;
									}
									else if(string[i] == '1')
									{
										temp = temp->right;
										i++;
									}
								}
								
								if(temp->left == NULL)
								{
								        
									printf("  %c ",temp->data.id);
								}	
								else
									printf("INVALID STRING\n");
							}
							
						}
						else
							printf("PLEASE BUILD THE TREE FIRST \n");
						break;
			case 5	:	break;
		}

	}while(sw != 5);
	return 0;
}
