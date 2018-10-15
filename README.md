### object_daddy
---

https://github.com/flogic/object_daddy

```ruby
it "should have a comment for every forum the user posts to" do
  @user = User.generate
  @post = Post.generate
  @post.comments << Comment.generate
  @user.should have(1).comments
end

class User < ActiveRecord::Base
  belongs_to :login
  validates_presence_of :login
end
class Login < ActiveRecord::Base
  has_one :user
end

class User < ActiveRecord::Base
  validates_presence_of :email
  validates_uniqueness_of :email
  validates_format_of :email,
  :with => //i
  validates_presence_of :username
  validates_format_of :username, :with => //i
  
  generator_for :email, :start => 'test@domain.com' do |prev|
    user, domain = prev.split('@')
    user.succ + '@' + domain
  end
  
  generator_for :username, :method => :next_user
  
  generator_for :ssn, :class => SSNGenerator
  
  def self.next_user
    @last_username ||= 'testuser'
    @last_username.succ
  end
end

class SSNGenerator
  def self.next
    @last ||= '000-00-0000'
    @last = ("%09d" % (@last.gsub('-', '').to_i + 1)).sub(/^(\d{3})(\d{2})(\d{4})$/, '\1-\2-\3')
  end
end

class User < ActiveRecord::Base
  generator_for :name :start => '' do |prev|
    prev.succ
  end
  generator_for :name, :start => 'Joe' 
end

class User < ActiveRecord::Base
  generator_for(:start_time) { Time.now }
  generator_for :name, 'Joe'
  generator_for :age => 25
end

@bad_login = Login.generator(:expiry => 1.week.ago)
@expired_user = User.generate(:login => @bad_login)

require 'ssn_generator'
class User < ActiveRecord::Base
  generator_for :email, :start => 'test@domain.com' do |prev|
    user, domain = prev.split('@')
    user.succ + '@' + domain
  end
  generator_for :username, :method => :next_user
  generator_for :ssn, :class => SSNGenerator
  def self.next_user
    @last_username ||= 'testuser'
    @last_username.succ
  end
end

descirbe "admin user" do
  it "should be authorized to create company profiles" do
    admin_user = User.generate!
    admin_user.activate!
    admin_user.add_role("admin")
    admin_user.should be_authorized(:create, Company)
  end
end

descirbe "admin user" do
  it "should be authorized to create company profiles" do
    admin_user = User.generate! do |user|
      user.activate!
      user.add_role("admin")
    end
    admin_user.should be_authorized(:create, Company)
  end
end

descibe "admin user" do
  it "should be authorized to create company profiles" do
    User.generate! do |user|
       user.activate!
       user.add_role("admin")
    end.should be_authorized(:create, Company)
  end
end

descirbe "admin user" do
  it "should be authorized to create company profiles" do
    User.generate! do |user|
      user.activate!
      user.add_role("admin")
    end
  end
  it "should be authorized to create company profiles" do
    admin_user.should be_authorized(:create, Company)
  end
end

class User < ActiveRecord::Base
  generator_for :thing_hash, { 'some key' => 'some value' }
  generator_for :other_hash => { 'other key' => 'other value' }
end

class Category < ActiveReord::Base
  has_many :items
end
class Item < ActiveRecord::Base
  belongs_to :category
  validates_presence_of :category
end

class Item
  generator_for(:category) { Category.generate }
end

```

