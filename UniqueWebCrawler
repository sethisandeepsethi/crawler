import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import java.io.IOException;
import java.util.HashSet;
import java.util.Set;
import java.util.concurrent.ConcurrentHashMap;

public class WebCrawler {
    private static final int MAX_DEPTH = 5; // Maximum depth to limit the crawl
    private static ConcurrentHashMap<String, Boolean> visitedUrls = new ConcurrentHashMap<>();
    private static Set<String> uniqueUrls = ConcurrentHashMap.newKeySet();

    public static void main(String[] args) {
        String url = "https://example.com"; // Replace with the starting website URL
        crawl(url, 0);
    }

    public static void crawl(String url, int depth) {
        if (depth > MAX_DEPTH || !uniqueUrls.add(url) || visitedUrls.putIfAbsent(url, true) != null) {
            return;
        }

        try {
            Document document = Jsoup.connect(url).get();
            System.out.println("Title: " + document.title());
            System.out.println("URL: " + url);

            Elements links = document.select("a[href]");
            links.parallelStream()
                    .map(link -> link.absUrl("href"))
                    .forEach(absoluteUrl -> crawl(absoluteUrl, depth + 1));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
