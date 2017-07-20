require 'rake/clean'

REVEALJS_VERION = "3.5.0"

CLOBBER << "html"

task default: %w[generate_book generate_slides]

desc "Crée le répertoire de destination des fichiers html"
directory "html"
file "html" do |html|
    tn = html.name
    sh <<~HEREDOC
        wget -qO- https://github.com/hakimel/reveal.js/archive/#{REVEALJS_VERION}.tar.gz | \
        tar --transform 's/^reveal.js-#{REVEALJS_VERION}/reveal.js/' -xz -C #{tn}/
    HEREDOC
    sh <<~HEREDOC
        sed -e 's/  font-size: ..px;/  font-size: 24px;/' \
        #{tn}/reveal.js/css/theme/black.css > #{tn}/reveal.js/css/theme/blackmy.css
    HEREDOC
end

desc "Crée le répertoire de destination des images"
directory "html/figs"
file "html/figs" => ["html"]
file "html/figs" do
    cp Dir["figs/*.png", "figs/*.svg"], "html/figs"
end

desc "Génère la version livre du cours"
task generate_book: %w[html/figs] do
    sh "asciidoctor -r asciidoctor-diagram -D html/ index.adoc"
end

desc "Génère les slides"
task generate_slides: %w[
    html/figs
    html/intro.html
]

rule '.html' => [
    proc { |tn| tn.sub(/\.html$/, '.adoc').sub(/^html\//, '') }
] do |t|
    sh "asciidoctor-revealjs -r asciidoctor-diagram -D html/ #{t.source}"
end

