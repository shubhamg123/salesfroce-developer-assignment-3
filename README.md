# salesfroce-developer-assignment-3
public class assignment3 {
    public static void q1()
    {  list<student__C> kl=[select id,name,STUDENT_NAME__c from student__c where STUDENT_NAME__c  like 's%'];
        integer i=0;
        
        for(student__C d:kl)
        {    
            d.STUDENT_NAME__C +=' test'+ ' '+ i;
            student__c o=new student__c(STUDENT_NAME__C='TEST' +i,batch__C='a085j000003asJIAAY');
            insert o;
            i++;
        }
      update kl;  
        System.debug(kl);
    }
 public static void q2()
 {
     list<Account> ds=[select id,account.name from account where id not in (select accountid from opportunity)];
     list<opportunity> n=new list<opportunity>();
     for(account f:ds)
     {
        opportunity js= new opportunity(accountid=f.id,name='testing',stagename='prospecting',closedate=date.TODAY());
         n.add(js);
         
     }
     insert n;
     system.debug(n);
 }
 public static void q3()
{
    account mj=[select id from account where name like 'sssd' ];
    delete mj;
    undelete[select id from account where Isdeleted=true and name like 'sssd' all rows];
        
    
        
}
 public static void q4()
{
    student__C sc=[select id,STUDENT_NAME__C,rating__c from student__c where STUDENT_NAME__C like  'shubham%' limit 1 ];
    sc.STUDENT_NAME__C ='shruti agarwal';
    sc.rating__C =100;
 database.update(sc);
    upsert sc;
}
    public static void q5()
{
    
    list <student__c> yt=[select id,name,student_name__c from student__C where student_name__c like '_r%'];
        delete yt;
    database.emptyRecycleBin( yt );
    date fd=date.newinstance(2022,03,21);
     date fd1=date.newinstance(2022,03,24);
    list<student__c> jh=[select name from student__C where isdeleted=true and LastModifieddate>:fd and LastModifieddate<:fd and createdbyid='0055j000004aAUcAAM' all rows ];
    undelete(jh);
}
    public static void q6()
{
    contact hg=new contact(AssistantName ='ff',lastname='bhargava');
    date dt=date.today();
    dt.addDays(7);
    account dd=new account(externalid__c=0001);//reference account object without any other field just externalid field
    hg.account=dd;
    account parenta =new account(name='shubham',externalid__C=0001);
    database.saveresult[] rt=database.insert(new sobject[] {parenta,hg});
    //check result
    for(integer i=0;i<rt.size();i++)
    {
        if(rt[i].issuccess())
        {
            system.debug('successfully created id:' +rt[i].getid());
        }
        else
        {
            system.debug('error;could not createsobject'+'for array element'+i+'.');
            system.debug('error was'+rt[i].geterrors()[0].getmessage()+'\n');
        }}}
    public static void q7()
    {    date dt=date.today();
        dt.addDays(7);
        list<account> fd=new list<account>();
     list<OpportunityLineItem> dd=new list<OpportunityLineItem>();
     list<opportunity> fd1=new list<opportunity>();
     list<contact> fd2=new list<contact>();
        for(integer i=1;i<=5;i++)
        {
            account temp=new account(name='test '+i);
            fd.add(temp);
          }
        database.saveresult[] rt= database.insert(fd);
        for(database.SaveResult s:rt)
        {
            if(s.issuccess())
            {
             integer i=0;
                system.debug('successfully created account record'+i+s.id);
                
               opportunity sc=new opportunity(name='opportunitywith account'+i,
                                              accountid=s.id,
                                              stagename='prospecting',
                                              closedate=dt,
                                              Pricebook2Id='01s5j00000AhfVPAAZ');
             
                contact fd6=new contact(lastname='testint'+i,accountid=s.getid());
                fd1.add(sc);
                fd2.add(fd6);
            }
        }
     database.saveresult[] rtx =database.insert(fd1);
     System.debug(rtx);
     for(database.saveresult gg:rtx){
         if(gg.isSuccess())
     {   
     
      system.debug(gg.id);
        opportunitylineitem v=addd.opp(gg.id);
               dd.add(v);
     }
   
     
     }
     insert dd;
     database.saveresult[] rtx1 =database.insert(fd2);
    }
  public static void q8()
    {
    list<Account> aclist = [Select id, name, (select id, name from opportunities) from account];
        List<Account> deletedAcList = new List<Account>();
        for(Account ac : aclist)
        {
            List<Opportunity> opList = ac.opportunities;
           if( opList.size() > 2)
           {
               deletedAcList.add(ac);
           }
            
            
        }
        delete deletedAcList;
    }
    public static void q9(sobject sd)
{ object type=sd.getsobjecttype();
 String n=String.valueOf(type);
 integer count=database.countQuery('select count() from '+ n);
    system.debug(count);
}
    public static void main()
{   list<student__C> gf=new list<student__C>();
    for(integer i=0;i<100;i++)
    {
        student__c ds=new student__c(student_name__c='test student'+i);
       gf.add(ds);
       }
   savepoint sp=database.setsavepoint();
    database.SaveResult[] nh =database.insert(gf);
 integer countseccess=0;
  for(database.saveresult sr:nh )
  {
      if(sr.issuccess())
      {
          countseccess++;
      }   
      
  }
    if(countseccess<80)
    {
        database.rollback(sp);
        system.debug('lesser than 80 successfull records were insert,so we  rolled back to previous state');
    }
 else{
     system.debug('successfully insert '+countseccess+'records'+'in student');
 }
}}
