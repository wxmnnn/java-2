实验目的
初步了解分析系统需求，从学生选课角度了解系统中的实体及其关系，学会定义类中的属性以及方法；
掌握面向对象的类设计方法（属性、方法）；
掌握类的继承用法，通过构造方法实例化对象；
学会使用super()，用于实例化子类；
掌握使用Object根类的toString（）方法,应用在相关对象的信息输出中。
业务要求
说明：学校有“人员”，分为“教师”和“学生”，教师教授“课程”，学生选择“课程”。从简化系统考虑，每名教师仅教授一门课程，每门课程的授课教师也仅有一位，每名学生选仅选一门课程。
实验要求
1.编写上述实体类以及测试主类（注意类之间继承关系的适用）
2.在测试主类中，实例化多个类实体，模拟学生选课操作、打印课程信息（信息包括：编号、课程名称、上课地点、时间、授课教师 等）；模拟学生退课操作，再打印课程信息。
程序：
package c;

public class ChooseCourse {
	public static void main(String[] args) {
        Student stu0 = new Student(1001,"Lily");
        Student stu1 = new Student(1002,"Eilly");
        Student stu2 = new Student(1003,"Floris");
        Student stu3 = new Student(1004,"HaHa");
        Course cour0 = new Course(001,"高数");
        Course cour1 = new Course(002,"线代");
        Course cour2 = new Course(003,"概率论");
        stu0.addCourse(cour0);
        stu0.addCourse(cour2);
        stu0.addCourse(cour1);
        stu1.addCourse(cour2);
        stu1.addCourse(cour0);
        stu2.addCourse(cour1);
        stu3.addCourse(cour0);
        stu3.addCourse(cour1);
        stu1.removeCourse(cour2);
        stu0.displayCourse();
        cour0.removeStudent(stu1);
        cour1.displayStudent();
    }


}





package c;

public class Course {
	private String courseName;
    private int courseId;
    private Teacher teacher;
    private float credit;
    private Student[] students;
    public Course(int courseId,String courseName,float credit,Teacher teacher) {
        super();
        this.courseId=courseId;
        this.courseName=courseName;
        this.credit=credit;
        this.setTeacher(teacher);
        students = new Student[30];
    }
    public Course(int courseId,String courseName,float credit) {
        super();
        this.courseId=courseId;
        this.courseName=courseName;
        this.credit=credit;
        students = new Student[30];
    }

    public Course(int courseId,String courseName) {
        super();
        this.courseId=courseId;
        this.courseName=courseName;
        students = new Student[30];
    }

    public Course() {
        super();
        students = new Student[30];
    }

    public void setId(int id){
        this.courseId=id;
    }
    public int getId(){
        return this.courseId;
    }
    public void setName(String name){
        this.courseName=name;
    }
    public String getName(){
        return this.courseName;
    }
    public void setCredit(float credit ){
        this.credit=credit;
    }
    public float getCredit(){
        return this.credit;
    }
    public Teacher getTeacher() {
        return teacher;
    }
    public void setTeacher(Teacher teacher) {
        this.teacher = teacher;
    }

    public boolean addStudent(Student stu){
        boolean flag = false;
        if(!isSelectedStudent(stu)&&isNullStudent(stu)){
            for(int i=0;i<students.length;i++){
                if(students[i]==null){
                    students[i]=stu;
                    flag=true;
                    break;
                }
            }
        }
        return flag;
    }
    
    public boolean removeStudent(Student stu){
        boolean flag=false;
        if(isSelectedStudent(stu)){
            for(int i=0;i<students.length;i++){
                if(students[i]==stu){
                    students[i]=null;
                    flag=true;
                    break;
                }
            }
        }
        return flag;
    }
   
    public void displayStudent(){
        System.out.println("选择的课程："+this.courseName+"的学生有:");
        for(Student s:students){
            if(s!=null){
                System.out.print(s.getStuName()+" ");
            }
        }
        System.out.println();
    }
   
    public boolean isSelectedStudent(Student stu){
        boolean flag=false;
        for(Student s:students){
            if(s==stu){
                flag=true;
                break;
            }
        }
        return flag;
    }
   
    public boolean isNullStudent(Student stu){
        boolean flag=false;
        for(Student s:students){
            if(s==null){
                flag=true;
                break;
            }
        }
        return flag;
    }
    public static void main(String[] args) {
      
    }


}







package c;

public class Student {
	private String stuName;
    private int stuId;
    private String major;
    private Course[] courses;
    //构造函数
    public Student() {//不要忘
        super();
        courses = new Course[3];
    }
    public Student(int stuId,String stuName) {
        super();
        this.stuId=stuId;
        this.stuName=stuName;
        courses = new Course[3];
    }
    public Student(int stuId,String stuName,String major) {
        super();
        this.stuId=stuId;
        this.stuName=stuName;
        this.major = major;
        courses = new Course[3];
    }
    //修改获取属性name,id,major
    public String getStuName() {
        return stuName;
    }

    public void setStuName(String stuName) {
        this.stuName = stuName;
    }
    public int getStuId() {
        return stuId;
    }
    public void setStuId(int stuId) {
        this.stuId = stuId;
    }
    public String getMajor() {
        return major;
    }
    public void setMajor(String major) {
        this.major = major;
    }
    //学生选课；
    public boolean addCourse(Course course){
        boolean flag=false;
        if(!isSelectedCourse(course)&&isNullCourse(course)){
            for(int i=0;i<this.courses.length;i++){
                if(courses[i]==null){
                    courses[i]=course;
                    course.addStudent(this);//课程也要添加学生
                    flag=true;
                    break;
                }
            }
        }
        return flag;
    }
    //学生移除课程
    public boolean removeCourse(Course course){
        boolean flag=false;
        if(isSelectedCourse(course)){
            for(int i=0;i<this.courses.length;i++){
                if(courses[i]==course){
                    courses[i]=null;
                    course.removeStudent(this);//在课程中移除学生
                    flag=true;
                    break;
                }
            }

        }
        return flag;
    }
    //显示学生所选的课程
    public void displayCourse(){
        System.out.println("学生"+this.stuName+"所选课程有：");
        for(Course c:courses){
            if(c!=null){
                System.out.print(c.getName()+" ");
            }
        }
        System.out.println();
    }

    //子方法1：课是否被选过
    public boolean isSelectedCourse(Course course){
        boolean flag=false;
        for(Course c:courses){
            if(c==course){
                flag=true;
                break;
            }
        }
        return flag;
    }
    //子方法2：学生是否还有选修课位置
    public boolean isNullCourse(Course course){
        boolean flag=false;
        for(Course c:courses){
            if(c==null){
                flag=true;
                break;
            }
        }
        return flag;
    }


}




package c;

public class Teacher {
    private int id;
    private String teacherName;
    public Course[] courses;
    //构造函数
    public Teacher() {
        super();
        courses= new Course[3];
    }
    public Teacher(int id,String teacherName){
        this.id=id;
        this.teacherName=teacherName;
        courses = new Course[3];
    }
    //修改或是添加属性
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getTeacherName() {
        return teacherName;
    }
    public void setTeacherName(String teacherName) {
        this.teacherName = teacherName;
    }


}

实验感受

    
    
    
