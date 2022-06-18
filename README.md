 
1	#include<stdio.h>	
2	#include<stdlib.h>	
3	#include<string.h>	
4	#define N 50	
5	#define M 30	
6	double currentincome=0;	
7	double currentexpense=0;	
8		
9	struct node	
10	{	
11	char date[M];	
12	double amount;	
13	char category[N];	
14	struct node *next;	
15	}*income=NULL,*expense=NULL;	
16		
17	struct record	
18	{	
19	double x,y;	
20	}*point=NULL;	
21		
22		
23	void create(char x[],double y,char z[],struct node **temp);	
24	void display(int a3);	
25	struct node *readnext(struct node *ptr,FILE *fpointer);	
26	void writeincome(struct node *ptr);	
27	void writeexpense(struct node *ptr);	
28	void deleterecord(struct node *ptr);	
29	struct node *readincome(struct node *ptr);	
30	struct node *readexpense(struct node *ptr);	
31	void write(struct record *point);	
32	struct record *readrecord();	
33		
34		
35	int main()	
36	{	
37	int option,value;	
38		
39	double b;	
40	char c[N],a[M];	
41	char s1[15],s2[15],s3[15];	
42		
43		
44		
45	if(fopen("Record.bin","rb")!=NULL)	
46	{	
47	point=readrecord();	
48	currentincome=point->x;	
49	currentexpense=point->y;	
50	}	
51		
52	if(fopen("myincome.bin","rb")!=NULL)	
53	{	
54	income=readincome(income);	
55	}	
56	if(fopen("myexpense.bin","rb")!=NULL)	
57	{	
58	expense=readexpense(expense);	
59	}	
60		
61		
62	do{	
63		
64	printf("	 	\n	" );
65	printf("	|	YOUR INCOME	=	%.2lf INR	\n ", currentincome);
 
66	printf("	|	YOUR EXPENSE  =	%.2lf INR	\n ", currentexpense);
67	printf("	|	YOUR BALANCE  =	%.2lf INR	\n ", currentincome-currentexpense);
68 printf("	| 	\n");
69	printf("ENTER THE OPTION FROM THE BELOW \n\n");
70	printf("1.INSERT INCOME \n");
71	printf("2.INSERT EXPENSE \n");
72	printf("3.VIEW INCOME RECORD \n");
73	printf("4.VIEW EXPENSE RECORD \n");
74	printf("5.EXIT\n");
75	scanf("%d",&option);
76	printf("\n\n\n"); 77
78 switch(option) 79 {
80 case 1:
81 printf("**************	ADD INCOME	*****************\n\n");
82 printf("Enter The Date(e.g day month year)\n"); 83 scanf("%s %s %s",s1,s2,s3);
84	strcpy(a,"");
85	strcat(a,s1);
86	strcat(a," ");
87	strcat(a,s2);
88	strcat(a," ");
89	strcat(a,s3);
90	printf("Enter The Amount\n");
91	scanf("%lf",&b);
92	printf("Enter the Category\n");
93	scanf("%s",c); 94
95
96
97
98	currentincome=currentincome+b;
99	create(a,b,c,&income);
100	writeincome(income); 101
102	break;
103	case 2:
104 printf("**************	ADD EXPENSE	*****************\n\n");
105 printf("Enter The Date(e.g day month year)\n"); 106
107 scanf("%s %s %s",s1,s2,s3);
108	strcpy(a,"");
109	strcat(a,s1);
110	strcat(a," ");
111	strcat(a,s2);
112	strcat(a," ");
113	strcat(a,s3); 114
115	printf("Enter The Amount\n");
116	scanf("%lf",&b);
117	printf("Enter The Category\n");
118	scanf("%s",c); 119
120
121	currentexpense=currentexpense+b;
122	create(a,b,c,&expense);
123	writeexpense(expense); 124
125	break;
126	case 3:
127	printf("*********	YOUR INCOME RECORD IS	*******\n\n");
128	display(3);
129	break;
 
130	case 4:
131	printf("*********	YOUR EXPENSE RECORD IS	*******\n\n");
132	display(4);
133	break;
134	case 5:
135	point=(struct record*)malloc(sizeof(struct record));
136	point->x=currentincome;
137	point->y=currentexpense;
138	write(point);
139	break;
140	default:
141	printf("WRONG OPTION SELECTED -Enter Valid Option");
142	break;
143 }
144 }while(option!=5); 145
146
147 return 0;
148 } 149
150 void create(char x[],double y,char z[],struct node **temp)
151 {
152	struct node *newnode,*ptr;
153	newnode=(struct node*)malloc(sizeof(struct node));
154	if(*temp==NULL)
155 {
156	strcpy(newnode->date,x);
157	newnode->amount=y;
158	strcpy(newnode->category,z);
159	newnode->next=NULL;
160	*temp=newnode;
161 }
162 else
163 {
164	ptr=*temp;
165	while(ptr->next!=NULL)
166 {
167 ptr=ptr->next;
168 }
169	strcpy(newnode->date,x);
170	newnode->amount=y;
171	strcpy(newnode->category,z);
172	newnode->next=NULL;
173	ptr->next=newnode;
174 }
175 } 176
177 void deleterecord(struct node *ptr)
178 {
179	struct node *freeme =ptr;
180	struct node *holdme=NULL;
181	while(freeme!=NULL)
182 {
183	holdme=freeme->next;
184	free(freeme);
185	freeme=holdme;
186 }
187 } 188
189 struct node *readnext(struct node *ptr,FILE *fpointer)
190 { 191
192 if(ptr==NULL)
193 {
194	ptr=(struct node *)malloc(sizeof(struct node));
195	fread(ptr,sizeof(struct node),1,fpointer);
 
196	ptr->next=NULL;
197 }
198 else
199 {
200	struct node *ptr1=ptr;
201	struct node *ptr2=(struct node *)malloc(sizeof(struct node));
202	while(ptr1->next!=NULL)
203 {
204 ptr1=ptr1->next;
205 }
206	fread(ptr2,sizeof(struct node),1,fpointer);
207	ptr1->next=ptr2;
208	ptr2->next=NULL;
209 }
210 return ptr;
211 } 212
213 struct node *readincome(struct node *ptr)
214 {
215	FILE *fpointer;
216	fpointer=fopen("myincome.bin","rb");
217	if(fpointer!=NULL)
218 {
219	deleterecord(ptr);
220	ptr=NULL;
221	fseek(fpointer,0,SEEK_END);
222	long filesize=ftell(fpointer);
223	rewind(fpointer);
224	int entries=(int)(filesize/(sizeof(struct node)));
225	for(int i=0;i<entries;i++)
226 {
227	fseek(fpointer,(sizeof(struct node)*i),SEEK_SET);
228	ptr=readnext(ptr,fpointer);
229 }
230 }
231 else
232 {
233 printf("ERROR IN OPENINNG FILE\n");
234 }
235 return ptr;
236 } 237 238
239 void display(int a3)
240 {
241	if(a3==3)
242	{
243
244	if(fopen("myincome.bin","rb")==NULL)
245	{
246	printf("NO RECORDS AVAILABLE\n\n");
247	printf(
" 	
_\n\n"); 248
249		}
250		else
251	{	
252		
253		struct node *ptr2=income;
254		while(ptr2!=NULL)
255		{
256		printf("Date: %s\nAmount: %.2lf INR\nCategory: %s\n\n",ptr2->date,ptr2->
257		amount,ptr2->category);
258		ptr2=ptr2->next;
259		}
 
260	printf(
" 	
_\n\n"); 261
262	}
263 }
264 else if(a3==4)
265	{
266
267	if(fopen("myexpense.bin","rb")==NULL)
268	{
269	printf("NO RECORDS AVAILABLE\n\n");
270	printf(
" 	
_\n\n");
271	}
272	else
273	{
274
275	//	expense=readexpense(expense);
276	struct node *ptr2=expense;
277	while(ptr2!=NULL)
278	{
279	printf("Date: %s\nAmount: %.2lf INR\nCategory: %s\n\n",ptr2->date, ptr2->
280	amount,ptr2->category);
281	ptr2=ptr2->next;
282	}
283	printf(
" 	
_\n\n"); 284
285
286	}
287
288	} 289
290 } 291
292 void writeincome(struct node *ptr)
293 {
294	FILE *fpointer;
295	fpointer=fopen("myincome.bin","wb");
296	if(fpointer!=NULL)
297 {
298	struct node *ptr1=ptr;
299	struct node *holdnext=NULL;
300	while(ptr1!=NULL)
301 {
302	holdnext=ptr1->next;
303	ptr1->next=NULL;
304	fseek(fpointer,0,SEEK_END);
305	fwrite(ptr1,sizeof(struct node),1,fpointer);
306	ptr1->next=holdnext;
307	holdnext=NULL;
308	ptr1=ptr1->next;
309 }
310	fclose(fpointer);
311	fpointer=NULL;
312	printf("\nINCOME SAVED SUCCESSFULLY\n\n");
313	printf(
" 	
_\n\n"); 314
315 }
316	else{
 
317	printf("\nCANNOT SAVE INCOME..TRY AGAIN\n");
318	printf(
" 	
_\n\n"); 319
320 }
321 } 322 323 324 325 326
327 void writeexpense(struct node *ptr)
328 {
329	FILE *fpointer;
330	fpointer=fopen("myexpense.bin","wb");
331	if(fpointer!=NULL)
332 {
333	struct node *ptr1=ptr;
334	struct node *holdnext=NULL;
335	while(ptr1!=NULL)
336 {
337	holdnext=ptr1->next;
338	ptr1->next=NULL;
339	fseek(fpointer,0,SEEK_END);
340	fwrite(ptr1,sizeof(struct node),1,fpointer);
341	ptr1->next=holdnext;
342	holdnext=NULL;
343	ptr1=ptr1->next;
344 }
345	fclose(fpointer);
346	fpointer=NULL;
347	printf("\nEXPENSE SAVED SUCCESSFULLY\n\n");
348	printf(
" 	
_\n\n"); 349
350 }
351	else{
352	printf("\nCANNOT SAVE EXPENSE..TRY AGAIN\n\n");
353	printf(
" 	
_\n\n"); 354
355 }
356 } 357 358
359 struct node *readexpense(struct node *ptr)
360 {
361	FILE *fpointer;
362	fpointer=fopen("myexpense.bin","rb");
363	if(fpointer!=NULL)
364 {
365	deleterecord(ptr);
366	ptr=NULL;
367	fseek(fpointer,0,SEEK_END);
368	long filesize=ftell(fpointer);
369	rewind(fpointer);
370	int entries=(int)(filesize/(sizeof(struct node)));
371	for(int i=0;i<entries;i++)
372 {
373	fseek(fpointer,(sizeof(struct node)*i),SEEK_SET);
374	ptr=readnext(ptr,fpointer);
375 }
376 }
 
377 else
378 {
379 printf("cannonot open file\n"); 380
381 }
382 return ptr;
383 } 384
385 void write(struct record *point)
386 {
387	FILE *fpointer;
388	fpointer=fopen("Record.bin","wb");
389	if(fpointer!=NULL)
390 { 391
392	fseek(fpointer,0,SEEK_END);
393	fwrite(point,sizeof(struct record),1,fpointer);}
394	else{
395	printf("FILEOPEN ERROR\n");
396 }
397	fclose(fpointer);
398	fpointer=NULL; 399
400 } 401
402 struct record *readrecord()
403 {
404 FILE *fpointer;
405 fpointer=fopen("Record.bin","rb");
406 struct record *ptr=NULL; 407
408 if(fpointer!=NULL)
409 { 410
411 fseek(fpointer,0,SEEK_SET); 412
413 ptr=(struct record *)malloc(sizeof(struct record));
414 fread(ptr,sizeof(struct record),1,fpointer); 415
416
417 }
418 else
419 {
420 printf("CANNOT OPEN FILE\n");
421 }
422 return ptr;
423 }

