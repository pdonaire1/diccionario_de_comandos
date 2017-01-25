# Upload files:
##[source](https://github.com/thoughtbot/paperclip#models)

**In Gemfile:** Add
```
gem "paperclip", "~> 5.0.0"
gem 'paperclip-rack', require: 'paperclip/rack'  # Optional if there are problems
gem 'cocaine', '0.5.8'  # Optional if there are problems
```

**In Migrations:**
```
class CreateWebAppFiles < ActiveRecord::Migration[5.0]
  def change
    create_table :web_app_files do |t|
      t.attachment :file
      ....
      t.timestamps
    end
  end
end
```

**In models:** 
If you want to add images and also files. 

You can read more [here](https://github.com/thoughtbot/paperclip#models)
```
has_attached_file :file, 
    :styles => lambda { |a| if a.content_type =~ %r{^(image|(x-)?application)/(x-png|pjpeg|jpeg|jpg|png|gif)$}
      { 
        :thumb => "100x100#",
        :medium => "300x300>",
      }
    else
      Hash.new
    end
    },:default_url => "/missing.png"
  
  validates :file, attachment_presence: true
  validates_with AttachmentPresenceValidator, attributes: :file

  do_not_validate_attachment_file_type :file
```
