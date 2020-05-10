# LinqQueries

Linq


1*

            var query = from p in db.Products
                        select  p;

            dataGridView1.DataSource = query.ToList();




2*

            //Select productID, ProductName, unitPrice from products

            var query = from p in db.Products
                        select new
                        {
                            p.productID,
                            p.ProductName,
                            p.UnitPprice
                        };


3*


            //Select  *  from products where ProductID = 1;

            var query = from p in db.Products
                        where p.productID == 1
                        select p;


4*
//Select  *  from products where supplierID = 5 and unitPrice > 20;

            var query = from p in db.Products
                        where p.supplierID == 5  && p.UnitPrice > 20
                        select p;



5*

//Select  *  from products where supplierID = 5 OR p.SupplierID = 6;

            var query = from p in db.Products
                        where p.supplierID == 5  || p.SupplierID == 6
                        select p;

            dataGridView1.DataSource = query.ToList();


6*

//Select  *  from products order by productID dec

            var query = from p in db.Products
                       orderby p.ProductID descending
                        select p;



7*

  //Select  *  from products order by categoryID , unitPrice desc

            var query = from p in db.Products
                       orderby p.CategoryID ascending , p.UnitPrice descending
                        select p;

8*

  //Select  top 10  from products

            var query = (from p in db.Products
                         select p ) .Take(10) ;

9*
 var query = (from p in db.Products
                         select p ).First();
//returns 1st recrd


10*
    //Select  top 10  from products order by pID

            var query = (from p in db.Products
                         orderby p.ProductID 
                         select p ).Take(10);

11*
  //Select  distinct  categoryID from products 

            var query = (from p in db.Products
                         select p.CategoryID ).Distinct();

12*

            //Select categoryID , count(categoryID) As newfield from products 

            var query = from p in db.Products
                        group p by p.CategoryID into g
                        select new {
                            CategoryId = g.Key ,
                            NewField = g.Count()
                        };

13*
//Select categoryID , avg(unitprice) as newfield from products groupBy categoryID

            var query = from p in db.Products
                        group p by p.CategoryID into g
                        select new {
                            CategoryId = g.Key ,
                            NewField = g.Average(k => k.UnitPrice)
                        };

14*
 //Select categoryID , sum(unitprice) as newfield from products groupBy categoryID

            var query = from p in db.Products
                        group p by p.CategoryID into g
                        select new {
                            CategoryId = g.Key ,
                            NewField = g.Sum(k => k.UnitPrice)
                        };


15*

            //Select * from productswhere categoryID = 1 Union select * from products where categoryID = 2

            var query = ( from p in db.Products
                         where p.CategoryID == 1
                         select p )
                         .Union
                        ( from m in db.Products
                         where m.CategoryID == 2
                         select m );


16*


            //Select A.ProductID, A.productName, B.CategoryID, B.CtegoryName 
            //from products A, Categories B
            // where A.categoryID = B.categoryID and supplierId = 1 

            var query = from p in db.Products
                          from m in db.Categories
                         where p.CategoryID == m.CategoryID
                          && p.SupplierID == 1
                        select new {
                            p.ProductID,
                            p.ProductName,
                            m.CategoryID,
                            m.CategoryName
                        };

17*

//Select * from products where productName like 'A%'

            var query = from p in db.Products
                        where p.ProductName.StartsWith("A")
                        select p;



18*
 //Select * from products where productName like 'A%' and customerName like 'P%';

            var query = from c in db.Customers
                        from p in db.Products
                        where c.ContactName.StartsWith("A")
                         && p.ProductName.StartsWith("P")
                        select new
                        {
                            name = c.ContactName,
                            p.ProductName
                        };

			



19* UPDATE :


  var cc = from c in db.Customers
                    where c.City.StartsWith("Paris")
                    select c;

            foreach (var cust in cc)
                cust.City = "PARIS";

            db.SubmitChanges();

            var cd = from c in db.Customers
                     where c.City.StartsWith("Paris")
                     select c;


20* INSERT:

Product newProduct = new Product();

            newProduct.ProductName = "RC helicopter";
            db.Products.InsertOnSubmit(newProduct);
            db.SubmitChanges();


21*  DELETE:

 var pp = from p in db.Products
                     where p.ProductName.Contains("helicopter")  //  where p.ProductName.Equals("helicopter")
                     select p;

            db.Products.DeleteAllOnSubmit(pp);

            db.SubmitChanges();
