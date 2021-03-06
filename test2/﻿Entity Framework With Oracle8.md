[Entity Framework With Oracle](http://www.cnblogs.com/Gyoung/archive/2013/02/04/2881747.html)
=============================================================================================

虽然EF6都快要出来了，但是对于Oracle数据库，仍然只能用DB first和Model First来编程，不能用Code First真是一个很大的遗憾啊。

好了，废话少说，我们来看看EF中是如何用DB first和Model First来对Oracle编程的。

首先我们要下载ODP.NET这个数据驱动程序，下载链接：[http://www.oracle.com/technetwork/topics/dotnet/index-085163.html](http://www.oracle.com/technetwork/topics/dotnet/index-085163.html)

安装成功后，我们在VS连接Oracle数据库时就可以选择ODP.NET了，如图：

**Model First**
---------------

模型优先是先建立数据模型，然后再根据模型生成相应的数据库脚本，然后再根据脚本生成数据库。

在项目中新增一个ADO.NET实体模型：OracleModel.edmx，选择“空模型”，再新新建两个实体：Destination与Lodging，如图：

为了看清这两个模型中属性的数据类型，我把他们生成的类也贴出来一下：

![](/pictures/)![](/pictures/)View Code

     public partial class Destination
        {
            public Destination()
            {
                this.Lodging = new HashSet<Lodging>();
            }
        
            public int DestinationId { get; set; }
            public string Name { get; set; }
            public string Country { get; set; }
            public byte Photo { get; set; }
            public string Description { get; set; }
        
            public virtual ICollection<Lodging> Lodging { get; set; }
        }

     public partial class Lodging
        {
            public int LodgingId { get; set; }
            public string Name { get; set; }
            public string Owner { get; set; }
            public bool IsResort { get; set; }
            public decimal MilesFromNearestAirport { get; set; }
            public int DestinationDestinationId { get; set; }
        
            public virtual Destination Destination { get; set; }
        }

实体模型的空白处，右键-属性，在打开的OracleModel模型属性窗口，设置一些属性，将DDL生成模板改成：**SSDLToOracle.tt (VS)**，数据库架构名称改成：**GYOUNG**(这是我自己测试的Oracle数据库的用户名，大家可根据自己的更改)，数据库生成工作流改成：**Generate Oracle Via T4 (TPT).xaml (VS)**

为了让EF更好的明白.NET中的数据类型与Oracle中数据类型间的对应关系。我们可以将下面的配置文件加到app.config中。

      <oracle.dataaccess.client>
        <settings>
          <add name="bool" value="edmmapping number(1,0)" />
          <add name="byte" value="edmmapping number(3,0)" />
          <add name="int16" value="edmmapping number(4,0)" />
          <add name="int32" value="edmmapping number(9,0)" />
          <add name="int64" value="edmmapping number(18,0)" />
        </settings>
      </oracle.dataaccess.client>

现在我们就可以生成数据库的相应脚本了。

在空白处右键，选择“根据模型生成数据库”

然后建立好数据连接，如图：

点击下一步，然后就会生成相应的数据脚本。

![](/pictures/)![](/pictures/)View Code

    -- Creating table 'Destinations'
    CREATE TABLE "GYOUNG"."Destinations" (
       "DestinationId" NUMBER(9,0) NOT NULL,
       "Name" NCLOB NOT NULL,
       "Country" NCLOB NOT NULL,
       "Photo" NUMBER(3,0) NOT NULL,
       "Description" NCLOB NOT NULL
    );

    -- Creating table 'Lodgings'
    CREATE TABLE "GYOUNG"."Lodgings" (
       "LodgingId" NUMBER(9,0) NOT NULL,
       "Name" NCLOB NOT NULL,
       "Owner" NCLOB NOT NULL,
       "IsResort" NUMBER(1,0) NOT NULL,
       "MilesFromNearestAirport" NUMBER(38,0) NOT NULL,
       "DestinationDestinationId" NUMBER(9,0) NOT NULL
    );


    -- --------------------------------------------------
    -- Creating all PRIMARY KEY constraints
    -- --------------------------------------------------

    -- Creating primary key on "DestinationId"in table 'Destinations'
    ALTER TABLE "GYOUNG"."Destinations"
    ADD CONSTRAINT "PK_Destinations"
       PRIMARY KEY ("DestinationId" )
       ENABLE
       VALIDATE;


    -- Creating primary key on "LodgingId"in table 'Lodgings'
    ALTER TABLE "GYOUNG"."Lodgings"
    ADD CONSTRAINT "PK_Lodgings"
       PRIMARY KEY ("LodgingId" )
       ENABLE
       VALIDATE;


    -- --------------------------------------------------
    -- Creating all FOREIGN KEY constraints
    -- --------------------------------------------------

    -- Creating foreign key on "DestinationDestinationId" in table 'Lodgings'
    ALTER TABLE "GYOUNG"."Lodgings"
    ADD CONSTRAINT "FK_DestinationLodging"
       FOREIGN KEY ("DestinationDestinationId")
       REFERENCES "GYOUNG"."Destinations"
           ("DestinationId")
       ENABLE
       VALIDATE;

    -- Creating index for FOREIGN KEY 'FK_DestinationLodging'
    CREATE INDEX "IX_FK_DestinationLodging"
    ON "GYOUNG"."Lodgings"
       ("DestinationDestinationId");

    -- --------------------------------------------------
    -- Script has ended
    -- --------------------------------------------------

我们只要将脚本到数据库中执行一下就可以生成相应的表了。分析一下生成的SQL语句，有主键，外键，但并没有为主键设置自增长。Oracle设置自增长也是一个很蛋疼的问题，要通过设置相应的Sequences和Triggers来实现，习惯了SQL SERVER的IDENTITY，对于这个还真不爽。这里我们不管它，就自己插入主键好了。下面是测试代码：

![](/pictures/)![](/pictures/)View Code

     using (OracleModelContainer context = new OracleModelContainer())
                {
                    var destination = new Destination
                    {
                        DestinationId=1,
                        Country = "Indonesia",
                        Description = "EcoTourism at its best in exquisite Bali",
                        Name = "Bali"
                    };
                    var lodging = new Lodging
                    {
                        LodgingId=1,
                        Owner="Jshon",
                        Name = "Top Notch Resort and Spa",
                        MilesFromNearestAirport = 30,
                        IsResort=true,
                        Destination=destination
                    };
                    context.Lodgings.Add(lodging);
                    context.SaveChanges();
                }

通过VS连接Oracle可以看到数据插入成功。

**DB First**
------------

DB First顾名思义就是在先建好数据库，再进行编程。我们新建一个项目，就以刚刚生成的那再张表来编程。

在新建项目中添加一个“ADO.NET 实体数据模型”：DBModel.edmx，选择“从数据库生成”

设置好连接串

选择表

点击完成，就会生成相应的模型。

我们来检索一下刚刚插入的数据。

![](/pictures/)![](/pictures/)View Code

     using (Entities context = new Entities())
                {
                    var des = context.Destinations.FirstOrDefault();
                    var log = context.Lodgings.FirstOrDefault();
                    Console.WriteLine("Lodging  Name:" + log.Name + " Owner:" + log.Owner);
                    Console.WriteLine("Destination   Name:" + des.Name + " Country:" + des.Country);
                }

结果如图。

PS：在DB First模式中，更要将Model First中所说的映射配置文件加入App.config中，不然很多数据类型映射会出错。

 

**伪Code First**
----------------

见我的另一篇博客：[Entity Framework Code First在Oracle下的伪实现](http://www.cnblogs.com/Gyoung/archive/2013/06/12/3132630.html)

 
