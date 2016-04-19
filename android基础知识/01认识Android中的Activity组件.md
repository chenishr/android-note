#####   启动另一个 Activity
1.  创建一个 Activity（IDE 会帮助完成相关工作）
2.  在某个事件实现代码中添加启动代码

        //启动另一个 Activity
        startActivity(new Intent(MainActivity.this,OtherActivity.class));
        
#####  在启动 Activity 时传递数据
*   传递简单的数据

    使用 Intent 来传递各种类型的数据  
    如：整型、整型数组、布尔类型、字符串类型、字符串数组类型...
    
    示例代码：
    
        //启动另一个 Activity ，并且传递个简单的数据
        //设置数据
        Intent  i   = new Intent(MainActivity.this,OtherActivity.class);
        i.putExtra("name","chenishr");
        startActivity(i);
        
        ------
        
        //获取上一个 Activity 传过来的数据
        Intent i    = getIntent();
        TextView    tv  = (TextView) findViewById(R.id.textView2);
        tv.setText(i.getStringExtra("name"));
        
*   传递数据包 Bundle

    Bundle 是各种数据类型的集合  
    
        //启动另一个 Activity ，并且以数据包的方式传递数据
        Intent  i   = new Intent(MainActivity.this,OtherActivity.class);
        
        //设置数据
        Bundle  b   = new Bundle();
        b.putString("name","CHENISHR");
        b.putInt("gender",0);
        //b.putString("nickName","chenishr");
        
        //i.putExtras(b);
        i.putExtra("data",b);
        
        startActivity(i);
        
        ------
        
        //获取上一个 Activity 传过来的数据包
        Intent i    = getIntent();
        TextView    tv  = (TextView) findViewById(R.id.textView2);
        //Bundle  data    = i.getExtras();
        Bundle  data    = i.getBundleExtra("data");
        tv.setText(String.format("name=%s,gender=%d,nickName=%s", data.getString("name"),data.getInt("gender"),data.getString("nickName","defaultValue")))
        
*   传递值对象

    值对象是指自定义的有数据类型的对象  
    该对象需要实现 Serializable 或者 Parcelable 接口
    
    +  首先看实现 Serializable 接口的值对象传值方式
    
        首先定义一个作为值对象使用的类，它须实现 Serializeable 接口  
        代码如下：
        
            public class User implements Serializable{
                private String  name;
                private int     gender;
                public String getName(){
                    return name;
                }
                
                public int getGender(){
                    return gender;
                }
            
                public void setName(String name){
                    this.name   = name;
                }
                
                public void setGender(int gender){
                    this.gender = gender;
                }
            
                public User(String name,int gender){
                    this.name   = name;
                    this.gender = gender;
                }
            }
            
            //然后设置传输数据
        
            //启动另一个 Activity ，并且以值对象的方式传递数据
            Intent  i   = new Intent(MainActivity.this,OtherActivity.class);
            
            //设置数据
            i.putExtra("user",new User("chenishr",0));
            
            startActivity(i);
            
            //最后就是在新启动的 Activity 中使用传过来的数据
        
            //获取上一个 Activity 传过来的值对象
            Intent i    = getIntent();
            TextView    tv  = (TextView) findViewById(R.id.textView2);
            
            User user       = (User) i.getSerializableExtra("user");
            
            tv.setText(String.format("name=%s,gender=%d", user.getName(),user.getGender()));
            
        
    
    
    +  现在来看实现 Parcelable 接口的值对象传值方式
    
        定义这种类型值对象的类时，需要重载两个方法，还要实现一个 Creator
        
            public class User implements Parcelable{
                private String  name;
                private int     gender;
                
                public String getName(){
                    return name;
                }
                
                public int getGender(){
                    return gender;
                }
                
                public void setName(String name){
                    this.name   = name;
                }
                
                public void setGender(int gender){
                    this.gender = gender;
                }
                
                public User(String name,int gender){
                    this.name   = name;
                    this.gender = gender;
                }
                
                @Override
                public int describeContents() {
                    return 0;
                }
                
                @Override
                public void writeToParcel(Parcel dest, int flags) {
                    dest.writeString(getName());
                    dest.writeInt(getGender());
                }
                
                public static final Creator<User> CREATOR = new Creator<User>() {
                    @Override
                    public User createFromParcel(Parcel source) {
                        return new User(source.readString(),source.readInt());
                    }
                    
                    @Override
                    public User[] newArray(int size) {
                        return new User[size];
                    }
                };
            }
            
            
            //然后设置传输数据
        
            //启动另一个 Activity ，并且以值对象的方式传递数据
            Intent  i   = new Intent(MainActivity.this,OtherActivity.class);
            
            //设置数据
            i.putExtra("user",new User("chenishr",0));
            
            startActivity(i);
            
            //最后就是在新启动的 Activity 中使用传过来的数据
        
            //获取上一个 Activity 传过来的值对象
            Intent i    = getIntent();
            TextView    tv  = (TextView) findViewById(R.id.textView2);
            
            User user       = i.getParcelableExtra("user");
            
            tv.setText(String.format("name=%s,gender=%d", user.getName(),user.getGender()));
            
            
#####   接收 Activity 返回的数据

