require 'rake/clean'
# rake/clean définit CLEAN (Rake::FileList) pour les fichiers temporaires
# et CLOBBER (Rake::FileList) pour les fichiers de sortie

chapters = %w[intro] # %w définit un tableau de chaînes

REVEALJS_VERION = "3.5.0"

# Array : https://ruby-doc.org/core-trunk/Array.html
# Rake::FileList : http://ruby-doc.org/stdlib-trunk/libdoc/rake/rdoc/Rake/FileList.html
chapter_files = Rake::FileList[chapters.map{|c| "#{c}/#{c}.adoc"}]
fig_dirs = Rake::FileList[chapters.map{|c| "#{c}/figs"}]

desc "Génère la version livre et la version slides"
task :default => [:generate_book, :generate_slides]
# :default est un symbole Ruby
# task <cible> => <dépendances>

desc "Initialise le répertoire de destination des fichiers html"
task :init_html => "html" do |t|
    tn = t.source
    sh <<~HEREDOC
        wget -qO- https://github.com/hakimel/reveal.js/archive/#{REVEALJS_VERION}.tar.gz | \
        tar --transform 's/^reveal.js-#{REVEALJS_VERION}/reveal.js/' -xz -C #{tn}/
    HEREDOC
    sh <<~HEREDOC
        sed -e 's/  font-size: ..px;/  font-size: 24px;/' \
        #{tn}/reveal.js/css/theme/black.css > #{tn}/reveal.js/css/theme/blackmy.css
    HEREDOC
end
# directory task pour créer le répertoire
directory "html"
# ajoute le répertoire html comme fichier de sortie
CLOBBER << "html"

desc "Initialise le répertoire des images"
task :init_figs => [:init_html, "html/figs"] do
    cp Dir["figs/*.png", "figs/*.svg"], "html/figs"
    fig_dirs.each do |d|
        cp Dir["#{d}/*.png", "#{d}/*.svg"], "html/figs"
    end
end
directory "html/figs"

desc "Génère la version livre du cours"
task :generate_book => %w[html/index.html]
# file task
file "html/index.html" => [:init_figs, :init_html, "index.adoc"] + chapter_files do
    sh "asciidoctor -r asciidoctor-diagram -D html/ index.adoc"
end

desc "Génère les slides"
task :generate_slides => %w[html/slides.html]
file "html/slides.html" => [:init_figs, :init_html, "slides.adoc"] + chapter_files do
    sh "asciidoctor-revealjs -r asciidoctor-diagram -D html/ slides.adoc"
end

