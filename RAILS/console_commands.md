# Generate models with foreign key
$ rails generate model photo album:references

This command will generate photos table with integer field album_id and also it will add index for this field automatically. Make sure in it by looking at generated migration:

    class CreatePhotos < ActiveRecord::Migration
      def change
        create_table :photos do |t|
          t.references :album

          t.timestamps
        end
        add_index :photos, :album_id
      end
    end
