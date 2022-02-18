from flask import session,Flask,render_template,request,redirect,url_for
import pymysql
import time


#Create the app
app=Flask(__name__)

#Apply secret key
app.secret_key = "super secret key"



@app.route('/')
def home():
    return render_template('Welcome.html')


@app.route('/admin_home')
def admin_home():
    # check the existance of session
    if 'usertype' in session:
        usertype = session['usertype']
        if usertype == 'admin':
            #Complete code of page
            return render_template('Adminhome.html')
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))

@app.route('/user_home')
def user_home():
    # check the existance of session
    if 'usertype' in session:
        usertype = session['usertype']

        if usertype == 'user':
            return render_template('usershome.html')
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))


@app.route('/auth_error')
def auth_error():
    return  render_template('Autherror.html')

@app.route('/login',methods=['GET','POST'])
def login():
    if request.method=='POST':
        email=request.form['T1']
        password=request.form['T2']
        conn = pymysql.connect(host="localhost", port=3306, user="root", passwd="", db="b296", autocommit=True)
        cur = conn.cursor()
        sql="select * from logindata where email='"+email+"' AND password='"+password+"'"
        cur.execute(sql)
        n=cur.rowcount
        print(n)
        if n==1:
            data=cur.fetchone()
            #find usertype
            usertype=data[2]

            #Create session
            session["usertype"]=usertype
            session["email"]=email

            #Open authorized page
            if usertype=='admin':
                return redirect(url_for('admin_home'))
            elif usertype=='user':
                return redirect(url_for('user_home'))
        else:
            return render_template('login.html',data1="Either email or password is incorrect")
    else:
        return render_template('login.html')

@app.route('/logout')
def logout():
    if 'usertype' in session:
        session.pop('usertype', None)
        session.pop('email', None)
        return redirect(url_for('login'))
    else:
        return redirect(url_for('login'))


@app.route('/admin_reg',methods=['GET','POST'])
def admin_reg():
    # check the existance of session
    if 'usertype' in session:
        usertype = session['usertype']
        if usertype == 'admin':
            # Complete code of page
            if request.method == 'POST':
                # Receive form data and save in table
                name = request.form["T1"]
                address = request.form["T2"]
                contact = request.form["T3"]
                email = request.form["T4"]
                password = request.form["T5"]
                usertype = "admin"

                # save data in tables
                conn = pymysql.connect(host="localhost", port=3306, user="root", passwd="", db="b296", autocommit=True)
                cur = conn.cursor()
                s1 = "insert into admindata values('" + name + "','" + address + "','" + contact + "','" + email + "')"
                s2 = "insert into logindata values('" + email + "','" + password + "','" + usertype + "')"

                cur.execute(s1);
                n = cur.rowcount
                cur.execute(s2)
                m = cur.rowcount

                msg = "Error: No data saved"
                if (n == 1 and m == 1):
                    msg = "Data save and login created"
                elif (n == 1):
                    msg = "Data saved but login not created"
                elif (m == 1):
                    msg = "Login created but data not saved"
                return render_template('AdminReg.html', kota=msg)
            else:
                return render_template('AdminReg.html')
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))



@app.route('/show_admins')
def show_admins():
    # check the existance of session
    if 'usertype' in session:
        usertype = session['usertype']
        if usertype == 'admin':
            # Complete code of page
            conn = pymysql.connect(host="localhost", port=3306, user="root", passwd="", db="b296", autocommit=True)
            cur = conn.cursor()
            sql = "select * from admindata"
            cur.execute(sql)
            n = cur.rowcount
            if n > 0:
                # Fetch the records from selected data
                table_data = cur.fetchall()
                # Render the data in view
                return render_template('showAdmin.html', data=table_data)
            else:
                return render_template('showAdmin.html', data1="No data found")
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))



@app.route('/user_reg',methods=['GET','POST'])
def user_reg():
    # check the existance of session
    if 'usertype' in session:
        usertype = session['usertype']
        if usertype == 'admin':
            # Complete code of page
            if request.method == 'POST':
                # Receive form data and save in table
                name = request.form["T1"]
                branch = request.form["T2"]
                rollno = request.form["T3"]
                contact = request.form["T4"]
                email = request.form["T5"]
                password = request.form['T6']
                usertype = "user"

                # save data in tables
                conn = pymysql.connect(host="localhost", port=3306, user="root", passwd="", db="b296", autocommit=True)
                cur = conn.cursor()
                s1 = "insert into userdata values('" + name + "','" + branch + "','" + rollno + "','" + contact + "','" + email + "')"
                s2 = "insert into logindata values('" + email + "','" + password + "','" + usertype + "')"

                cur.execute(s1);
                n = cur.rowcount
                cur.execute(s2)
                m = cur.rowcount

                msg = "Error: No data saved"
                if (n == 1 and m == 1):
                    msg = "Data save and login created"
                elif (n == 1):
                    msg = "Data saved but login not created"
                elif (m == 1):
                    msg = "Login created but data not saved"
                return render_template('userreg.html', kota=msg)
            else:
                return render_template('userreg.html')
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))




@app.route('/show_users')
def show_users():
    # check the existance of session
    if 'usertype' in session:
        usertype = session['usertype']
        if usertype == 'admin':
            # Complete code of page
            conn = pymysql.connect(host="localhost", port=3306, user="root", passwd="", db="b296", autocommit=True)
            cur = conn.cursor()
            sql = "select * from userdata"
            cur.execute(sql)
            n = cur.rowcount
            if n > 0:
                # Fetch the records from selected data
                table_data = cur.fetchall()
                # Render the data in view
                return render_template('showusers.html', data=table_data)
            else:
                return render_template('showusers.html', data1="No data found")
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))


@app.route('/edit_user',methods=['GET','POST'])
def edit_user():
    # check the existance of session
    if 'usertype' in session:
        usertype = session['usertype']
        if usertype == 'admin':
            # Complete code of page
            if request.method == 'POST':
                name = request.form['H1']
                branch = request.form['H2']
                rollno = request.form['H3']
                contact = request.form['H4']
                email = request.form['H5']

                return render_template('Edituser.html', name=name, branch=branch, rollno=rollno, contact=contact,
                                       email=email)
            else:
                return redirect(url_for('show_users'))
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))

@app.route('/edit_user1',methods=['GET','POST'])
def edit_user1():
    # check the existance of session
    if 'usertype' in session:
        usertype = session['usertype']
        if usertype == 'admin' \
                       '':
            # Complete code of page
            if request.method == 'POST':
                name = request.form['T1']
                branch = request.form['T2']
                rollno = request.form['T3']
                contact = request.form['T4']
                email = request.form['T5']
                conn = pymysql.connect(host="localhost", port=3306, user="root", passwd="", db="b296", autocommit=True)
                cur = conn.cursor()
                sql = "update userdata set name='"+name+"',branch='"+branch+"',rollno='"+rollno+"',contact='"+contact+"' where email='"+email+"'"
                cur.execute(sql)
                n = cur.rowcount
                if n > 0:
                    return render_template('Edituser1.html',data="Changes saved")
                else:
                    return render_template('Edituser1.html', data="Try again")
            else:
                return redirect(url_for('show_users'))
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))


@app.route('/admin_changepass',methods=['GET','POST'])
def admin_changepass():
    # check the existance of session
    if 'usertype' in session:
        usertype = session['usertype']
        e1=session['email']
        if usertype == 'admin':
            # Complete code of page
            if request.method=='POST':
                oldpass=request.form['T1']
                newpass=request.form['T2']
                conn = pymysql.connect(host="localhost", port=3306, user="root", passwd="", db="b296", autocommit=True)
                cur = conn.cursor()
                sql = "update logindata set password='"+newpass+"' where email='" + e1 + "' AND password='"+oldpass+"'"
                cur.execute(sql)
                n = cur.rowcount
                msg="Cannot change password"
                if n==1:
                    msg="Password changed."
                return render_template('Adminchangepassword.html',data=msg)
            else:
                return render_template('Adminchangepassword.html')
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))

@app.route('/admin_profile',methods=['GET','POST'])
def admin_profile():
    # check the existance of session
    if 'usertype' in session:
        usertype = session['usertype']
        e1=session['email']
        if usertype == 'admin':
            # Complete code of page
            conn = pymysql.connect(host="localhost", port=3306, user="root", passwd="", db="b296", autocommit=True)
            cur = conn.cursor()

            if request.method == 'POST':
                return render_template('Adminprofile.html')
            else:
                sql = "select * from admindata where email='"+e1+"'"
                cur.execute(sql)
                n = cur.rowcount
                if n==1:
                    data=cur.fetchone()
                    return render_template('Adminprofile.html',data=data)
                else:
                    return render_template('Adminprofile.html', data1="No data found")
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))



@app.route('/user_profile',methods=['GET','POST'])
def user_profile():
    # check the existance of session
    if 'usertype' in session:
        usertype = session['usertype']
        e2=session['email']
        if usertype == 'user':
            # Complete code of page
            conn = pymysql.connect(host="localhost", port=3306, user="root", passwd="", db="b296", autocommit=True)
            cur = conn.cursor()

            if request.method == 'POST':
                return render_template('userprofile.html')
            else:
                sql = "select * from userdata where email='"+e2+"'"
                cur.execute(sql)
                n = cur.rowcount
                if n==1:
                    data=cur.fetchone()
                    return render_template('userprofile.html',data=data)
                else:
                    return render_template('userprofile.html', data1="No data found")
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))

@app.route('/myquestions')
def myquestions():
    # check the existance of session
    if 'usertype' in session:
        usertype = session['usertype']
        e1=session['email']
        if usertype == 'user':
            conn = pymysql.connect(host="localhost", port=3306, user="root", passwd="", db="b296", autocommit=True)
            cur = conn.cursor()
            sql = "select * from qbank where qby='"+e1+"' order by qdate desc"
            cur.execute(sql)
            n = cur.rowcount
            if n>0:
                data=cur.fetchall()
                return render_template('MyQuestions.html',data=data)
            else:
                return render_template('MyQuestions.html', data1="No question found")
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))

@app.route('/ask',methods=['GET','POST'])
def ask():
    # check the existance of session
    if 'usertype' in session:
        usertype = session['usertype']
        e1=session['email']
        if usertype == 'user':
            if request.method=='POST':
                subject=request.form['T1']
                question=request.form['T2']
                t=(int)(time.time()) #System time in seconds
                qby=e1
                conn = pymysql.connect(host="localhost", port=3306, user="root", passwd="", db="b296", autocommit=True)
                cur = conn.cursor()
                sql="insert into qbank(qsub,question,qdate,qby) values('"+subject+"','"+question+"',"+str(t)+",'"+qby+"')"
                cur.execute(sql)
                n=cur.rowcount
                msg="Error : Cannot upload question. Try again"
                if n==1:
                    msg="Your question is saved."
                return render_template('ask.html',msg=msg)
            else:
                return render_template('ask.html')
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))


@app.route('/show_otherquestions',methods=['GET','POST'])
def show_otherquestions():
    if 'usertype' in session:
        usertype=session['usertype']
        e1=session['email']
        if usertype=='user':
            conn=pymysql.connect(host="localhost",passwd="",port=3306,user="root",db="b296",autocommit=True)
            cur=conn.cursor()
            sql="select*from qbank where qby <>'"+e1+"' order by qdate desc"
            cur.execute(sql)
            n=cur.rowcount
            if n>0:
                data=cur.fetchall()
                return render_template('showallquestions.html',data=data)
            else:
                return render_template('showallquestions.html',data1="no questions found")
        else:
            return redirect(url_for('auth _error'))
    else:
        return redirect(url_for('auth_error'))

@app.route('/my_questions',methods=['GET','POST'])
def my_questions():
    if 'usertype' in session:
        usertype=session['usertype']
        e1=session['email']
        if usertype=='user':
            conn=pymysql.connect(host="localhost",passwd="",port=3306,user="root",db="b296",autocommit=True)
            cur=conn.cursor()
            sql="select*from qbank where qby='"+e1+"' order by qdate desc"
            cur.execute(sql)
            n=cur.rowcount
            if n>0:
                data=cur.fetchall()
                return render_template('myquestions.html',data=data)
            else:
                return render_template('myquestions.html',data1="no questions found")
        else:
            return redirect(url_for('auth _error'))
    else:
        return redirect(url_for('auth_error'))

    @app.route('/solve', methods=['GET', 'POST'])
    def solve():
        # check the existance of session
        if 'usertype' in session:
            usertype = session['usertype']
            e1 = session['email']
            if usertype == 'user':
                if request.method == 'POST':
                    qid = request.form['H1']
                    conn = pymysql.connect(host="localhost", port=3306, user="root", passwd="", db="b296",
                                           autocommit=True)
                    cur = conn.cursor()
                    sql = "select * from qbank where qid=" + qid
                    cur.execute(sql)
                    n = cur.rowcount
                    if n > 0:
                        data = cur.fetchone()
                        sql1 = "select * from solution where qid=" + qid
                        cur.execute(sql1)
                        m = cur.rowcount
                        if m > 0:
                            soldata = cur.fetchall()
                            return render_template('solve.html', data=data, soldata=soldata)
                        else:
                            return render_template('solve.html', data=data)
                    else:
                        return render_template('solve.html', data1="No question found")
                else:
                    return redirect(url_for('show_questions'))
            else:
                return redirect(url_for('auth_error'))
        else:
            return redirect(url_for('auth_error'))

    @app.route('/solve1', methods=['GET', 'POST'])
    def solve1():
        # check the existance of session
        if 'usertype' in session:
            usertype = session['usertype']
            e1 = session['email']
            if usertype == 'user':
                if request.method == 'POST':
                    solution = request.form['T1']
                    qid = request.form['H1']
                    t = (int)(time.time())
                    solby = e1
                    conn = pymysql.connect(host="localhost", port=3306, user="root", passwd="", db="b296",
                                           autocommit=True)
                    cur = conn.cursor()
                    sql = "insert into solution(qid,solution,soldate,solby) values(" + qid + ",'" + solution + "'," + str(
                        t) + ",'" + solby + "')"
                    cur.execute(sql)
                    n = cur.rowcount
                    if (n == 1):
                        return render_template('solve1.html', data="Solution Uploaded")
                    else:
                        return render_template('solve1.html', data="Try Again")
                else:
                    return redirect(url_for('show_questions'))
            else:
                return redirect(url_for('auth_error'))
        else:
            return redirect(url_for('auth_error'))

@app.route('/show_questions_general')
def show_questions_general():
    if 'usertype' in session:
        usertype=session['usertype']
        q1=session['qid']
        if usertype=='admin':
            if request.method=='POST':
                conn=pymysql.connect(host="localhost",port=3306,user="root",db="b296",passwd="")
                cur=conn.cursor()
                sql="select* from qbank where qid='"+q1+"' order by qdate desc"
                cur.execute(sql)
                n=cur.rowcount
                if (n==1):
                    return render_template('showallquestions.html',data="question found")
                else:
                    return render_template('showallquestions.html',data="no question found")
            else:
                return redirect(url_for('show_questions_general'))
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))


@app.route('/delete_questions')
def delete_questions():
    if 'usertype' in 'session':
        usertype=session['usertype']
        q1=session['qid']
        if usertype == 'admin':
            if request.method == 'POST':
                conn = pymysql.connect(host="localhost", port=3306, user="root", db="b296", passwd="")
                cur = conn.cursor()
                sql1 = "select* from qbank where qid='" + q1 + "' order by qdate desc"
                cur.execute(sql1)
                sql2="select* from solution where qid='"+q1+"'"
                n=cur.rowcount
                m=cur.rowcount
                if (n == 1):
                    return render_template('delete.html', data="question found")
                else:
                    return render_template('delete.html', data="no question found")
            else:
                return redirect(url_for('delete_questions'))
        else:
            return redirect(url_for('auth_error'))
    else:
        return redirect(url_for('auth_error'))




if __name__=="__main__":
    app.run(debug=True)


