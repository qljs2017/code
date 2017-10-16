# code

Comment: The following Java function was written for my last school project, which is to build a website with Android app that is similar to FabFlix.com. After writing these codes, I finally understand how to display the content from databse onto a webpage. It is not complicated as some dynamic programming algorithms codes sample, but it was like opening a window to me. It is just a tiny part of the project, but somehow I feel it is important to me. The whole project was built by MySQL, Java(servlet), Tomcat, and AWS. Finally, the website funstions as same as the Fabflix.com. (however, with a simpler UI) For the contribution part, even though it is a team assignment,   I have done it all by myself(under the Instructor's instruction), because my teammate was focusing on his Math courses in order to get a higher GPA. However, I feel this project has many valuable information and doing all the work will not hurt me but help me to improve. 

public void doGet(HttpServletRequest request, HttpServletResponse response)
        throws IOException, ServletException
    {
        String loginUser = "mytestuser";
        String loginPasswd = "mypassword";
        String loginUrl = "jdbc:mysql://localhost:3306/moviedb";
        response.setContentType("text/html");    // Response mime type
        PrintWriter out = response.getWriter();
        
        try
           {
              //Class.forName("org.gjt.mm.mysql.Driver");
              Class.forName("com.mysql.jdbc.Driver").newInstance();

              Connection dbcon = DriverManager.getConnection(loginUrl, loginUser, loginPasswd);
              // Declare our statement
              Statement statement = dbcon.createStatement();

              String email = request.getParameter("email");
              String pd = request.getParameter("password");
	      
              String query = "SELECT password,id from customers where email = '" + email + "'";

              // Perform the query
              ResultSet rs = statement.executeQuery(query);
              
              int correct = 0;
              int cid = 0;
              int noemail = 1;
              
              while (rs.next()){
            	  noemail = 0;
                  if (pd.equals(rs.getString(1))){
                	  cid = rs.getInt(2);
                      correct = 1;
                      break;
                  }
              }
              
            
             
      		if (correct != 0)
    		//if(true)
            {    
   			 response.getWriter().print("true");

    			c_session = request.getSession(true);
    			c_session.setAttribute("cid", cid);
    			c_session.setAttribute("connection", dbcon);
    			c_session.setAttribute("cart", new HashMap<String, Integer>());
    			request.setAttribute("success", "success");
    			request.setAttribute("login", "");
    			//response = "success "+ response;
    			request.getRequestDispatcher("main.jsp").forward(request, response);
    			
    			
    		}else{
       		  response.getWriter().print("false");
    		}
              rs.close();
              statement.close();
              //dbcon.close();
            }
        catch (SQLException ex) {
   		 response.getWriter().print("false");
              while (ex != null) {
                    System.out.println ("SQL Exception:  " + ex.getMessage ());
                    ex = ex.getNextException ();
                }  // end while
            }  // end catch SQLException

        catch(java.lang.Exception ex)
            {
			 response.getWriter().print("false");

                System.out.println("<HTML>" +
                            "<HEAD><TITLE>" +
                            "MovieDB: Error" +
                            "</TITLE></HEAD>\n<BODY>" +
                            "<P>SQL error in doGet: " +
                            ex.getMessage() + "</P></BODY></HTML>");
                return;
            }
         out.close();
    }
