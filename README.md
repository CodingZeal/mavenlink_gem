!()[http://project-management.com/wp-content/uploads/2013/09/Mavenlink-Logo.jpg]

## Mavenlink API

Uses "ActiveRecord" style of accessing your records. You can also perform any custom request.
Utilizes (Brainstem API Adaptor gem)[http://github.com/einzige/brainstem-ruby]
See (Specification first)[lib/config/specification.yml]

### Style #1

Pretty similar to ActiveRecord. I do use ActiveModel to deal with records.
Pretty similar to Stripe API (people like it).

```ruby
Mavenlink.oauth_token = token

workspace = Mavenlink::Workspace.new(title: 'New workspace')
workspace.save # will call "create" and store record in Mavenlink db

workspace.participants # will return participants, will do http API call if not "included"
workspace.participants # now it returns cached value
workspace.participants(true) # flushes association cache
workspace.reload # reloads from remote host

# same thing...
Mavenlink::Workspace.create(title: 'new workspace')

Mavenlink::Workspace.find(9)

Mavenlink::Workspace.scoped.include(:participants).search('Sergei Workspace') # similar to activerecord scope...

Mavenlink::Workspace.scoped.search('Sergei Workspace').each do |workspace|
  if workspace.valid?
    workspace.destroy
  end
end

# ... find more in code, in specs...
```

### Style #2

Old fashioned way, slightly improved...

```ruby
client = Mavenlink::Client.new(oauth_token: '...')

client.workspaces.each { |workspace| do_something(workspace) }

client.workspaces.include('participants')
client.workspaces.include('participants, creator')
client.workspaces.include('participants', 'creator')
client.workspaces.include(['participants', 'creator'])
client.workspaces.include(:participants, :creator)

client.workspaces.find(2) # Returns one BrainstemAdaptor::Record

client.workspaces      # Returns sort of "active record relation collection"
client.workspaces.to_a # Same as calling #to_a on activerecord scope
client.workspaces.all  # Same as calling #to_a on activerecord scope

client.workspaces.page(3).per_page(20)
client.workspaces.limit(100).offset(10)
client.workspaces.filter(by_something: true)
client.workspaces.search('some text')

client.workspaces.order(:updated_at)
client.workspaces.order('updated_at:desc')
client.workspaces.order(:updated_at, :desc)
client.workspaces.order(:updated_at, 'ASC')
client.workspaces.order(:updated_at, true)

client.workspaces.create(title: 'New workspace')
client.workspaces.only(8).update(title: 'new title')
client.workspaces.only(8).delete

# ... too much to describe ... see the code
```

### Other examples

```ruby
Mavenlink.client.users
client.get('/custom_path', {param: 'anything'})
client.post('/custom_path', {param: 'anything'})
client.put('/custom_path', {param: 'anything'})
client.delete('/custom_path', {param: 'anything'})
```
