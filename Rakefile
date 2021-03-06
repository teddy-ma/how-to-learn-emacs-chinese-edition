require 'bundler/setup'
require 'asciidoctor'

SOURCES = %w(docs/how-to-learn-emacs-html.asc docs/how-to-learn-emacs-pdf.asc)

desc "Run build task"
task :default => :build

desc "Build books"
task :build => [:clean] do
  SOURCES.each do |source|
    doc = Asciidoctor.load_file source
    docname = doc.attributes['docname']
    version = doc.attributes['revnumber']
    filename = [docname, version].compact.join('-')
    build_dir = "build/#{docname}"

    # Build pdf
    sh "bundle exec asciidoctor-pdf -r asciidoctor-pdf-cjk-kai_gen_gothic -a pdf-style=KaiGenGothicCN -D #{build_dir} -o #{filename}.pdf #{source}"

    # Build html
    sh "bundle exec asciidoctor -D #{build_dir}/#{filename} -o index.html #{source}"
    cp_r 'images', "#{build_dir}/#{filename}"
  end
end

desc "Clean build results"
task :clean do
  rm_r Dir['build/*']
end
